#!groovy

pipeline {
    agent any

    environment {
    }

    options {
        disableConcurrentBuilds()
        timestamps()
        timeout(time: 15, unit: 'MINUTES')
        buildDiscarder(
            logRotator(numToKeepStr: '20')
        )
    }

    stages {

        stage('deploy develop') {
            when {
                branch 'develop'
            }
            steps {
							echo "develop"
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


