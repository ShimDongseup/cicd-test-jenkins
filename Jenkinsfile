def oDockImage // 스테이지 간 변수 공유를 위해 상단에 선언

pipeline {
    agent any

    environment {
        strDockerImage = "shimdongseup/cicd-test:0.1"
    }

    stages {
        stage('Github Pull') {
            steps {
                git branch: 'main', 
                    url: 'https://github.com/ShimDongseup/cicd-test-jenkins.git'
            }
        }

        stage('Docker Image Build') {
            steps {
                // 변수 할당을 위해 script 블록 사용
                script {
                    oDockImage = docker.build(strDockerImage, "-f Dockerfile .")
                }
            }
        }

        stage('Docker Image Push') {
            steps {
                script {
                    // docker-auth라는 ID의 자격 증명이 Jenkins에 등록되어 있어야 합니다.
                    docker.withRegistry('', 'docker-auth') {
                        oDockImage.push()
                    }
                }
            }
        }
        
        stage('Deploy Server') {
            steps {
                sshagent(credentials: ['Deploy-Privatekey']) {
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@3.38.104.39 docker container rm -f sampleweb"
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@3.38.104.39 docker run -d -p 80:80 --name sampleweb ${strDockerImage}"
                }
            }
        }
    }
}