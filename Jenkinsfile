pipeline {
    agent any

    enviroment {
        IMAGE_NAME = "pistionhead/petrescue-app"
        TAG = "${BUILD_NUMBER}"
    }

    stages {
        stage('CLONE REPO'){
            steps {
                echo "cloning repo.....📦📦📦"
                git 'https://github.com/VARUNx96/PetRescue_Local_Deployment'
            }
        }
        stage('BUILD DOCKER IMAGE'){
            steps {
                echo "building image...🧱🧱🧱"
                sh 'docker build -t $IMAGE_NAME:$TAG .'
            }
        }
        stage('DOCKER HUB LOGIN'){
            steps {
                withCredentials([usernamePassword(credentialsid: 'dockerhub-cred',
                usernameVariable: 'USER',
                passwirdVariable: 'PASS')]) {
                    echo "logging into docker...🐳🐳🐳"
                    sh 'docker login -u $USER -p $PASS'
                }
            }
        }
        stage('PUSH DOCKER IMAGE'){
            steps{
                echo "pushing image...🫸🫸🫸"
                sh 'docker push $IMAGE_NAME:$TAG'
            }
        }
        stage('DEPLOYMENT K8s'){
            steps{
                sh '''
                kubectl set image deployment/petrescue-app \
                petrescue-app=$IMAGE_NAME:$TAG \
                -n petrescue-ns
                '''
            }
        }
    }
}
