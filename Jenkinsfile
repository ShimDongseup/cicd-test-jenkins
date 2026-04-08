pipeline {
    agent any

    stages {
        stage('Github Pull') {
            steps {
                // 스마트 따옴표(‘ ’)를 표준 따옴표(' ')로 수정
                git branch: 'main', 
                    url: 'https://github.com/ShimDongseup/cicd-test-jenkins.git'
            }
        }
        stage('Git clone end') {
            steps {
                // 스마트 따옴표(“ ”)를 표준 따옴표(" ")로 수정
                sh 'touch cicd_test.txt'
                sh 'echo "git clone end!" > cicd_test.txt'
            }
        }
    }
}
