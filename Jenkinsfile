pipeline {
    agent any

    parameters {
        choice(name: 'DEPLOY_ENV', choices: ['blue', 'green'], description: 'Choose the environment to deploy')
        choice(name: 'DOCKER_TAG', choices: ['blue', 'green'], description: 'Docker tag to use for image')
        booleanParam(name: 'SWITCH_TRAFFIC', defaultValue: false, description: 'Switch traffic to selected env')
    }

    environment {
        IMAGE_NAME = "rineeth05/bankapp"
        TAG = "${params.DOCKER_TAG}"
        KUBE_NAMESPACE = "webapps"
        SCANNER_HOME = tool 'sonar-scanner'
    }

    stages {

        stage('Git Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/Rineeth-R/DevOps-Prod-BlueGreen-Pipeline.git'
            }
        }

        stage('Maven Build') {
            steps {
                script {
                    echo "Cleaning old build files"
                    sh 'chmod -R u+w target || true'
                    sh 'rm -rf target || true'
                    echo "Building with Maven"
                    sh 'mvn clean package -DskipTests'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                catchError(buildResult: 'UNSTABLE', stageResult: 'FAILURE') {
                    withSonarQubeEnv('sonar') {
                        withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
                            sh """
                                ${SCANNER_HOME}/bin/sonar-scanner \
                                -Dsonar.projectKey=nodejsmysql \
                                -Dsonar.projectName=nodejsmysql \
                                -Dsonar.sources=. \
                                -Dsonar.java.binaries=target \
                                -Dsonar.host.url=$SONAR_HOST_URL \
                                -Dsonar.login=$SONAR_TOKEN
                            """
                        }
                    }
                }
            }
        }

        stage('Trivy FS Scan') {
            steps {
                sh "trivy fs --format table -o fs.html ."
            }
        }

        stage('Docker Build & Push') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred') {
                        sh """
                            docker build -t ${IMAGE_NAME}:${TAG} .
                            docker push ${IMAGE_NAME}:${TAG}
                        """
                    }
                }
            }
        }

        stage('Trivy Image Scan') {
            steps {
                sh "trivy image --format table -o image.html ${IMAGE_NAME}:${TAG}"
            }
        }

        stage('Deploy MySQL') {
            steps {
                script {
                    withKubeConfig(
                        clusterName: 'cluster-1',
                        credentialsId: 'k8-token',
                        namespace: "${KUBE_NAMESPACE}",
                        serverUrl: 'https://22CA5F1CF767AC0698F7135F3B2A6E67.sk1.eu-north-1.eks.amazonaws.com'
                    ) {
                        sh "kubectl apply -f mysql-ds.yml -n ${KUBE_NAMESPACE}"
                    }
                }
            }
        }

        stage('Deploy SVC') {
            steps {
                script {
                    withKubeConfig(
                        clusterName: 'cluster-1',
                        credentialsId: 'k8-token',
                        namespace: "${KUBE_NAMESPACE}",
                        serverUrl: 'https://22CA5F1CF767AC0698F7135F3B2A6E67.sk1.eu-north-1.eks.amazonaws.com'
                    ) {
                        sh """
                            if ! kubectl get svc bankapp-service -n ${KUBE_NAMESPACE}; then
                                kubectl apply -f bankapp-service.yml -n ${KUBE_NAMESPACE}
                            fi
                        """
                    }
                }
            }
        }

        stage('Deploy App (Blue/Green)') {
            steps {
                script {
                    def deploymentFile = (params.DEPLOY_ENV == 'blue') ? 'app-deployment-blue.yml' : 'app-deployment-green.yml'

                    withKubeConfig(
                        clusterName: 'cluster-1',
                        credentialsId: 'k8-token',
                        namespace: "${KUBE_NAMESPACE}",
                        serverUrl: 'https://22CA5F1CF767AC0698F7135F3B2A6E67.sk1.eu-north-1.eks.amazonaws.com'
                    ) {
                        sh "kubectl apply -f ${deploymentFile} -n ${KUBE_NAMESPACE}"
                    }
                }
            }
        }

        stage('Switch Traffic') {
            when {
                expression { return params.SWITCH_TRAFFIC }
            }
            steps {
                script {
                    def newEnv = params.DEPLOY_ENV
                    withKubeConfig(
                        clusterName: 'cluster-1',
                        credentialsId: 'k8-token',
                        namespace: "${KUBE_NAMESPACE}",
                        serverUrl: 'https://22CA5F1CF767AC0698F7135F3B2A6E67.sk1.eu-north-1.eks.amazonaws.com'
                    ) {
                        sh """
                            kubectl patch service bankapp-service \
                            -p '{"spec": {"selector": {"app": "bankapp", "version": "${newEnv}"}}}' \
                            -n ${KUBE_NAMESPACE}
                        """
                    }
                    echo "Switched traffic to: ${newEnv}"
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                script {
                    def env = params.DEPLOY_ENV
                    withKubeConfig(
                        clusterName: 'cluster-1',
                        credentialsId: 'k8-token',
                        namespace: "${KUBE_NAMESPACE}",
                        serverUrl: 'https://22CA5F1CF767AC0698F7135F3B2A6E67.sk1.eu-north-1.eks.amazonaws.com'
                    ) {
                        sh """
                            kubectl get pods -l version=${env} -n ${KUBE_NAMESPACE}
                            kubectl get svc bankapp-service -n ${KUBE_NAMESPACE}
                        """
                    }
                }
            }
        }
    }

    post {
        cleanup {
            cleanWs()
        }
    }
}
