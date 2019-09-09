#!groovy

def getHost(){
    def remote = [:]
    remote.name = 'ott-beta'
    remote.host = '35.243.153.80'
    remote.user = 'shinetechsoftwarechina_gmail_com'
    remote.port = 22
    remote.identityFile = '/var/lib/jenkins/.ssh/id_rsa'
    remote.allowAnyHosts = true
    return remote
}

pipeline {
    agent any

    environment {
        def server = ''
        def path = '/var/www/ott-develop'
        def branch = 'develop'
        def port = 3000			
    }

    stages {
        stage('deploy develop') {
            when {
                branch 'develop'
            }
            steps {
                script {
                    server = getHost()
                }
            }

            steps{
                script {
                    sshCommand remote: server, command: """
                    cd ${path} && git checkout ${branch} && git pull
                    """
                }
            }

            steps {
                script {
                    sshCommand remote: server, command: """
                    cd ${path} && npm install
                    """
                }
            }

            steps {
                script {
                    sshCommand remote: server, command: """
                    cd ${path} && pm2 reload keystone.js && sleep 15
                    """
                }
            }

            steps {
                script {
                    sshCommand remote: server, command: """
                    if netstat -nelt | grep -q ${port}; then echo "success"; else exit 1; fi
                    """
                }
            }
        }

        stage('deploy master') {
            when {
                branch 'master'
            }
            steps {
                echo "master"
            }
        }
    }
}


