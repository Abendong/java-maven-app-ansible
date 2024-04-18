
pipeline {
    agent any
    stages {
        stage("copy files to ansible server") {
            steps {
                script {
                    echo "copying all neccesary files to ansible"
                    sshagent(['ansible-server-key']) {
                        sh "scp -o StrictHostKeyChecking=no ansible/* root@157.230.20.102:/root"
                        withCredentials([sshUserPrivateKey(credentialsId: 'ec2-server-key', KeyFileVariable: 'keyfile', usernameVariable: 'user')]) {
                            sh "scp $keyfile root@157.230.20.102:root/ssh-key.pem"
                        }                           
                    }
                }
            }
        }
    }   
}