pipeline {
    agent any

    environment {
        DOCKER_TAG = getDockerTag()
        MY_ID = 'avinkovic'
        GIT_REPO = 'https://github.com/vinkovicA/vjezba.git'
    }

    stages {
        stage('Pull git repo') {
            steps {
                sh "git clone ${GIT_REPO}"
            }
        }

        stage('Test1') {
            steps {
                sh "echo vršim prvi test.."
            }
        }

        stage('Test2') {
            steps {
                sh "echo vršim drugi test.."
            }
        }

        stage('Build Docker image') {
            steps {
                echo 'Starting to build the docker image..'
                sh "docker build . -t ${TAGGED_IMG}"
            }
        }
        
        stage('Publish image to Docker Hub') {
            steps {
                sh "docker image push ${MY_ID}/${TAGGED_IMG}"
            }
        }

        stage('Deploy to cluster') {
            steps {
                sh "docker image push ${MY_ID}/${TAGGED_IMG}"
            }
        }
    }
}

def getDockerTag() {
    //def tag = sh script:
}
