pipeline {
    agent {
        label 'docker'
    }
    stages {
        stage('Source') {
            steps {
                //git 'https://github.com/mhidalgop89/unir-cicd.git'
                sh 'git clone https://github.com/mhidalgop89/unir-cicd.git'
            }
        }
        stage('Build') {
            steps {
                echo 'Building stage!'
                echo 'build essential'
                sh 'pwd'
                //sh 'sudo apt-get install build-essential'
                sh 'make build'
            }
        }
        stage('Unit tests') {
            steps {
                sh 'make test-unit'
                archiveArtifacts artifacts: 'results/*.xml'
            }
        }
        stage('Unit tests api') {
            steps {
                sh 'make test-api'
                archiveArtifacts artifacts: 'results/*.xml'
            }
        }
        stage('Unit tests e2e') {
            steps {
                sh 'make test-e2e'
                archiveArtifacts artifacts: 'results/*.xml'
            }
        }
    }
    post {
        always {
            junit 'results/*_result.xml'
            cleanWs()
        }
        success {
            echo 'Ejecuciòn exitosa'
        }
        failure {
            echo 'Error en el pipeline'
            mail to: 'mario.hidalgo89@hotmail.com', body: "<b>Pipeline - ERROR</b><br>Proyecto: ${env.JOB_NAME} <br>N&uacute;mero compilaci&oacute;n: ${env.BUILD_NUMBER} <br> URL de compilaci&oacute;n: ${env.BUILD_URL}", charset: 'UTF-8', mimeType: 'text/html', subject: "ERROR CI: Nombre del proyecto -> ${env.JOB_NAME}";
        }
    }
}
