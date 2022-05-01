pipeline{
    agent any
    environment {
        dockerImage='v1'
        registry = "dhyeyadesai/dhyeya_calc"
        registryCredential = 'docker_d'
    }
    stages {
        stage('Clone GitHub Repo') {
            steps {
                git url: 'https://github.com/DhyeyaDesai/dhyeya_calc.git', branch: 'main'
                // credentialsId: 'github_token'
            }
        }
        stage('Test'){
            steps {
                sh 'mvn clean test'
            }
        }
        stage('Build'){
            steps {
                sh 'mvn install'
            }
        }
        stage('Building our image') {
            steps{
                script {
                    dockerImage = docker.build registry + ":$dockerImage"
                }        
            }
        }
        stage('Deploy our image') {
            steps{
                script {
                    docker.withRegistry( '', registryCredential ) {
                        dockerImage.push()  
                    }
                }
            }
        }
        stage('Cleaning up') {
            steps{
                sh "docker rmi $registry:v1"
            }
        }
        stage('Ansible Deploy') {
             steps {
                  ansiblePlaybook colorized: true, disableHostKeyChecking: true, installation: 'Ansible', inventory: 'inventory', playbook: 'p2.yml'
             }
        }
    }
}
