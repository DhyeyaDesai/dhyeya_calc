pipeline{
    agent any
    environment {
        dockerImage='v1'
        registry = "preyasgarg/calc"
        registryCredential = 'docker-login'
    }
    stages {
        stage('Clone GitHub Repository') {
            steps {
                git url: 'https://github.com/preyasgarg/MyCalculator.git'//, branch: 'master',
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
