pipeline {
    agent any
    stages {
        stage('Git Clone') {
            steps {
                git url: 'https://github.com/advit2012/star-agile-banking-finance.git/', branch: 'master'
                sh 'mvn clean install'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // Define the image name with a unique tag
                    env.IMAGE_NAME = "advit2012/akshatnewimg1july:v2"
                    sh "docker build -t ${env.IMAGE_NAME} ."
                    sh 'docker images'
                }
            }
        }
        stage('Docker Login and Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'advit2012', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh "docker push ${env.IMAGE_NAME}"
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    def dockerrm = 'sudo docker rm -f My-first-containe2211 || true'
                    def dockerPull = "sudo docker pull ${env.IMAGE_NAME}"  // Use the correct image tag
                    def dockerCmd = "sudo docker run -itd --name My-first-containe2211 -p 8084:8091 ${env.IMAGE_NAME}"
                    
                    sshagent(['sshkeypair']) {
                        // Remove existing container if exists
                        sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.6.255 ${dockerrm}"
                        // Pull the latest Docker image
                        sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.6.255 ${dockerPull}"
                        // Run the new container
                        sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.6.255 ${dockerCmd}"
                    }
                }
            }
        }
    }
}
