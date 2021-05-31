pipeline {
    agent any

    environment {
        //DOCKER_TAG = getDockerTag()
        DEPENDENCY_TYPE = 'services-dependent'
        DOCKERHUB_PATH = "avinkovic/${DEPENDENCY_TYPE}"

        GIT_REPO = 'https://github.com/vinkovicA/vjezba.git'

        SRV1_VERSION = '1.0.0'
        SRV2_VERSION = '1.0.0'

        TAGGED_SRV1_IMG = "service1:${SRV1_VERSION}"
        TAGGED_SRV2_IMG = "service2:${SRV2_VERSION}"

        SRV1_CONFIG = 'srv1-config.yaml'
        SRV2_CONFIG = 'srv2-config.yaml'
    }

    stages {
        stage('Pull git repo') {
            steps {
                sh "git clone ${GIT_REPO}"
            }
        }

        stage('Build Docker image for service1') {
            steps {
                echo 'Starting to build the docker image..'
                sh "docker build . -t ${TAGGED_SRV1_IMG}"
            }
        }

        stage('Build Docker image for service2') {
            steps {
                sh 'echo Starting to build the docker image..'
                sh "docker build . -t ${TAGGED_SRV2_IMG}"
            }
        }

        stage('Unit test') {
            steps {
                sh 'echo Running unit test..'
            }
        }

        stage('Integration test') {
            steps {
                sh 'echo running integration test..'
            }
        }

        stage('Publish images to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-token', usernameVariable: 'USERNAME', paswordVariable: 'PASSWORD')]) {
                    sh  "docker login -u '<$USERNAME>' -p '<$TOKEN>'"
                }

                sh 'echo Publishing service1 image to Docker Hub'
                sh "docker image push ${DOCKERHUB_PATH}/${TAGGED_SRV1_IMG}"

                sh 'echo Publishing service2 image to Docker Hub'
                sh "docker image push ${DOCKERHUB_PATH}/${TAGGED_SRV2_IMG}"
            }
        }

        stage('Deploy containers to K8s cluster for staging') {
            steps {
                sh 'Setting up namespaces..'
                sh 'kubectl create namespace staging'
                sh 'kubectl create namespace production'

                sh 'echo Deploying service1..'
                sh 'helm install service1-prod helmchart -n staging -f srv1-values.yaml'
                
                sh 'echo Deploying service2..'
                sh 'helm install service2-prod helmchart -n staging -f srv2-values.yaml'

                echo 'Setting up Ingress for staging..'
                sh 'kubectl apply -f ingress.yaml'
            }
        }

        stage('Smoke test') {
            steps {
                sh 'echo running smoke tests..'
            }
        }

        stage('End to end test') {
            steps {
                sh 'echo running e2e test..'
            }
        }

        stage('Deploy containers to K8s cluster for production') {
            steps {
                sh 'echo Deploying service1.. (using same values as for staging)'
                sh 'helm install service1-prod helmchart -n production -f srv1-values.yaml'

                sh 'echo Deploying service2.. (using same values as for staging)'
                sh 'helm install service2-prod helmchart -n production -f srv2-values.yaml'

                echo 'Setting up Ingress for prod..'
                sh 'kubectl apply -f ingress.yaml'
            }
        }

    }

    post {
        success {
            sh 'echo Build successful!'
        }

        failure {
            sh 'echo Build unsuccessful!'
        }
    }
}
