pipeline {
    agent any

    tools {
        nodejs 'node'
         dockerTool 'docker'
    }

    environment {
        IMAGE_NAME = 'kitakiv/react-app'
        IMAGE_TAG  = "${BUILD_NUMBER}"
    }

    stages {
        stage('Build') {
            steps {
                sh 'chmod +x scripts/build.sh'
                sh 'scripts/build.sh'
            }
        }

        stage('Test') {
            steps {
                sh 'chmod +x scripts/test.sh'
                sh 'scripts/test.sh'
            }
        }

        stage('Docker Image') {
            agent {
                kubernetes {
                    yamlFile './kaniko.yaml'
                }
            }
            steps {
                  container('kaniko') {
                     withCredentials([usernamePassword(credentialsId: 'docker-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh """
                /kaniko/executor \
                  --context `pwd` \
                  --dockerfile `pwd`/dockerfile \
                  --destination ${IMAGE} \
                  --oci-layout-path /kaniko/output \
                  --insecure-pull \
                  --skip-tls-verify \
                  --destination ${IMAGE} \
              """
                    }
                  }
            }
        }

        stage('Deploy to Minikube') {
            steps {
                sh "sed -i 's|IMAGE_PLACEHOLDER|${IMAGE_NAME}:${IMAGE_TAG}|g' k8s/deployment.yaml"
                sh 'kubectl apply -f k8s/deployment.yaml'
                sh 'kubectl rollout status deployment/my-app'
            }
        }
    }

    post {
        always {
            sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG} || true"
        }
    }
}
