pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/NikhilPatro123/zomato-Web-App.git'  //main
            }
        }
        stage('Build Docker Image') {
            steps {
                sh "docker build -t nikhilpatro05/zomato ."
            }
        }
        stage('Push Docker Image') {
            steps {
                withDockerRegistry([ credentialsId: '56a01b85-2652-4a67-b5bb-2198d15dda77', url: 'https://index.docker.io/v1/' ]) {
                    sh "docker push nikhilpatro05/zomato"
                }
            }
        }
    }
}
