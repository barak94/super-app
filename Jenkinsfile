pipeline {
    agent {
        label 'Docker'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/barak94/super-app.git'
            }
        }
        stage('Build') {
            steps {
                sh 'docker compose build'
            }
        }
        stage('Run & Test the Containers') {
            steps {
                sh 'docker compose up -d'
                sh 'sleep 5'
                sh 'curl http://localhost:80'
                sh 'curl http://localhost:3000/super-app'
                sh 'docker ps'
                sh 'docker compose down --volumes'
            }
        }
        stage('Upload to DockerHub') {
            steps {
                // tag images
                sh "docker tag final_project_pipeline-node bezalel1/super_app_node:${env.BUILD_NUMBER}"
                sh "docker tag final_project_pipeline-php bezalel1/super_app_php:${env.BUILD_NUMBER}"
                withCredentials(bindings:[usernamePassword(credentialsId: 'docker-hub', passwordVariable: 'pass', usernameVariable: 'user')]) {
                    // push images
                    sh "docker login -u $user -p $pass"
                    sh "docker push bezalel1/super_app_node:${env.BUILD_NUMBER}"
                    sh "docker push bezalel1/super_app_php:${env.BUILD_NUMBER}"
                }
                // delete images
                sh "docker rmi bezalel1/super_app_node:${env.BUILD_NUMBER}"
                sh "docker rmi bezalel1/super_app_php:${env.BUILD_NUMBER}"
            }
        }
    }
}
