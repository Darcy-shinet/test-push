#!groovy

def getHost(name, host, user, port, file){
    def remote = [:]
    // remote.name = 'ott-beta'
    // remote.host = '35.243.153.80'
    // remote.user = 'shinetechsoftwarechina_gmail_com'
    // remote.port = 22
    // remote.identityFile = '/var/lib/jenkins/.ssh/id_rsa'
    remote.name = name
    remote.host = host
    remote.user = user
    remote.port = port
    remote.identityFile = file
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
                    def name = 'ott-beta'
                    def host = '35.243.153.80'
                    def user = 'shinetechsoftwarechina_gmail_com'
                    def port = 22
                    def file = '/var/lib/jenkins/.ssh/id_rsa'
                    server = getHost(name, host, user, port, file)
                }
                script {
                    sshCommand remote: server, command: """
                    cd ${path} && git checkout ${branch} && git pull
                    """
                }
                script {
                    sshCommand remote: server, command: """
                    cd ${path} && npm install
                    """
                }
                script {
                    sshCommand remote: server, command: """
                    cd ${path} && pm2 reload keystone.js && sleep 15
                    """
                }
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


