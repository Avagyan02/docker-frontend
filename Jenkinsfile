pipeline {
    agent any
    
    environment {
        MY_VARIABLE = sh(returnStdout: true, script: 'git rev-parse --short=7 HEAD').trim()
    }

    triggers {
        GenericTrigger(
            token: 'docker-frontend'
        )
    }
    
    stages {
        stage('Extract Payload Hash') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'jenkins-environments', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        echo "Username: ${env.USERNAME}"
                        echo "Password: ${env.PASSWORD}"
                    }
                }

                sh 'cd /var/jenkins_home/workspace | ls -la' 
                sh 'docker build -t docker-frontend .'
                sh "docker tag docker-frontend samavgn02/docker-frontend:${env.MY_VARIABLE}"
                sh "docker login -u ${env.USERNAME} -p ${env.PASSWORD}"
                sh "docker push samavgn02/docker-frontend:${env.MY_VARIABLE}"
            }
        }
    }
}
