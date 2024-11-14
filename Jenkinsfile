pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'Git-Cred', url: 'https://github.com/Harshavardhana1154/star-agile-banking-finance.git']])
                echo 'GitHub URL checkout'
            }
        }
        
        stage('Code Compiling') {
            steps {
                echo 'Starting compilation'
                sh 'mvn compile'
            }
        }
        
        stage('Code Testing') {
            steps {
                sh 'mvn test'
            }
        }
        
        stage('Quality Assurance') {
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
        }
        
        stage('Code Packaging') {
            steps {
                sh 'mvn clean package'
            }
        }
        
        stage('Run Dockerfile') {
            steps {
                sh 'docker build -t harshavardhana08/bankingimg:1 .'
            }
        }
        
        stage('Docker Login') {
            steps {
                withCredentials([string(credentialsId: 'dockerhubpass', variable: 'DOCKER_PASSWORD')]) {
                    sh 'docker login -u harshavardhana08 -p $DOCKER_PASSWORD'
                }
            }
        }
        
        stage('Docker Push') {
            steps {
                withCredentials([string(credentialsId: 'dockerhubpass', variable: 'DOCKER_PASSWORD')]) {
                    sh 'docker push harshavardhana08/bankingimg:1'
                }
            }
        }
        
        stage('Deployment Using Ansible') {
            steps {
                ansiblePlaybook(become: true, becomeUser: 'root', credentialsId: 'ansible', disableHostKeyChecking: true, inventory: '/etc/ansible/hosts', playbook: 'ansible-playbook.yml')
            }
        }
    }
}
