pipeline {
    agent any
    environment {
        REGISTRE_CREDENTIALS = credentials('docker-hub')
    }
    stages {
        stage('Tester') {
            steps {
                echo "Tests de la revision ${env.GIT_COMMIT}"
                sh 'docker build --target test -t meteo-api:test-${BUILD_NUMBER} .'
            }
        }
        stage('Construire') {
            steps {
                sh 'docker build -t gakh00/meteo-api:${BUILD_NUMBER} -t gakh00/meteo-api:latest .'
            }
        }
        stage('Publier') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'REGISTRE_USER', passwordVariable: 'REGISTRE_PASS')]) {
                    sh 'echo $REGISTRE_PASS | docker login -u $REGISTRE_USER --password-stdin'
                    sh 'docker push gakh00/meteo-api:${BUILD_NUMBER}'
                    sh 'docker push gakh00/meteo-api:latest'
                }
            }
        }
    }
    post {
        always {
            sh 'docker logout'
            sh 'docker image prune -f'
        }
    }
}