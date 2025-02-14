pipeline {
    agent any
    stages {
        stage('Git Clone') {
            steps {
                git url: 'https://github.com/kalpeshdharkar/php-project/', branch: "master"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t kalpeshdharkar/2febimg:v1 .'  // Fixed the tag format
                    sh 'docker images'
                }
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo 'Logging in as $USER'"
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh 'docker push kalpeshdharkar/2febimg:v1'  // Fixed the tag format
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    def dockerrm = 'sudo docker rm -f My-first-containe2211 || true'
                    def dockerCmd = 'sudo docker run -itd --name My-first-containe2211 -p 8083:80 kalpeshdharkar/2febimg:v1'
                    
                    sshagent(['sshkeypair']) {
                        sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.23.188 ${dockerrm}"
                        sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.23.188 ${dockerCmd}"
                    }
                }
            }
        }
    }
}
