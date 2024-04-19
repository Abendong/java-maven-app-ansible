
pipeline {
    agent any
    environment {
        ANSIBLE_SERVER = "165.22.77.145"
    }
    stages {
        stage("copy files to ansible server") {
            steps {
                script {
                    echo "copying all neccesary files to ansible"
                    sshagent(['ansible-server-key']) {
                        sh "scp -o StrictHostKeyChecking=no ansible/* root@${ANSIBLE_SERVER}:/root"

                        withCredentials([sshUserPrivateKey(credentialsId: 'ec2-server-key', keyFileVariable: 'keyfile', usernameVariable: 'user')]) {
                            sh 'scp $keyfile root@$ANSIBLE_SERVER:/root/ssh-key.pem'
                        }                           
                    }
                }
            }
        }
        stage("execute ansible playbook") {
            steps {
                script{
                    echo "calling ansible playbook to configure ec2 instance"
                    def remote = [:]
                    remote.name = "ansible-server"
                    remote.host = ANSIBLE_SERVER
                    remote.allowAnyHosts = true

                    withCredentials([sshUserPrivateKey(credentialsId: 'ansible-server-key', keyFileVariable: 'keyfile', usernameVariable: 'user')]) {

                        remote.user = user
                        remote.identityFile = keyfile
                        sshCommand remote. remote, command: "ansible-playbook my-playbook.yaml"

                    }
                }
            }
        }
    }   
}