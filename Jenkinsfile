
pipeline {
    agent any
    stages {
        stage("copy files to ansible server") {
            steps {
                script {
                    echo "copying all neccesary files to ansible"
                    sshagent(['ansible-server-key']) {
                        sh "scp -o StrictHostKeyChecking=no ansible/* root@165.22.77.145:/root"

                        withCredentials([sshUserPrivateKey(credentialsId: 'ec2-server-key', keyFileVariable: 'keyfile', usernameVariable: 'user')]) {
                            sh "scp $[keyfile] root@165.22.77.145:root/ssh-key.pem"
                        }                           
                    }
                }
            }
        }
    }   
}