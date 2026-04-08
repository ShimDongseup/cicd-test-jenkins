pipeline {
    agent any

    environment {
        strDockerImage = "shimdongseup/cicd-test:0.1"
    }

    stages {
        stage('Github Pull') {
            steps {
                // 스마트 따옴표(‘ ’)를 표준 따옴표(' ')로 수정
                git branch: 'main', 
                    url: 'https://github.com/ShimDongseup/cicd-test-jenkins.git'
            }
        }
        stage('Docker Image Build') {
            script {

                oDockImage = docker.build(strDockerImage,"-f Dockerfile .")
            }
        }
        stage('Docker Image Push') {
            steps {
                docker.withRegistry('', 'docker-auth') {
                    oDockImage.push()
                }
            }
        }
        
        stage('Deploy Server') {
            steps{
                sshagent(credentials:['Deploy-Privatekey']){
                sh "scp -o StrictHostKeyChecking=no index.html ubuntu@3.38.104.39:/home/ubuntu"
                sh "ssh -o StrictHostKeyChecking=no ubuntu@3.38.104.39 sudo cp /home/ubuntu/index.html /var/www/html/"

                }
            }
        }
    }
}
