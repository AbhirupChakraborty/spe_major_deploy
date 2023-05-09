pipeline {
    agent any

    environment {
        IMAGE_NAME=''
    }

    stages {
        stage('Clone configuration repository') {
            steps {
                checkout([$class: 'GitSCM', 
                          branches: [[name: '*/master']], 
                          userRemoteConfigs: [[url: 'https://github.com/AbhirupChakraborty/spe_major_deploy.git']]])
            }
        }

        stage('Build and push frontend image') {
            steps {
                checkout([$class: 'GitSCM', 
                          branches: [[name: '*/master']], 
                          userRemoteConfigs: [[url: 'https://github.com/AbhirupChakraborty/SecuritySuvidha_frontend.git']]])

                script {
                    IMAGE_NAME=docker.build "abhirup18/spe_frontend"
                    docker.withRegistry('','docker-login'){
                      IMAGE_NAME.push()
                    }
                }
            }
        }

        stage('Build and push backend image') {
            steps {
                checkout([$class: 'GitSCM', 
                          branches: [[name: '*/master']], 
                          userRemoteConfigs: [[url: 'https://github.com/AbhirupChakraborty/SecuritySuvidha_backend.git']]])

                script {
                    IMAGE_NAME=docker.build "abhirup18/spe_backend"
                    docker.withRegistry('','docker-login'){
                      IMAGE_NAME.push()
                    }
                }
            }
        }

        stage('Deploy application using Ansible') {
            steps {
                sh "cd ansible && ansible-playbook -i inventory deploy.yml"
            }
        }
    }
}