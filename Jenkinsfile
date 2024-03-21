pipeline {
    agent any
    
    environment {
        MY_VARIABLE = sh(returnStdout: true, script: 'git rev-parse --short=7 HEAD').trim()
        DOCKERHUB_USERNAME = 'samavgn02'
        DOCKERHUB_PASSWORD = 'Avagyan2002'
    }

    triggers {
        GenericTrigger(
            token: 'jenkins-token'
        )
    }
    
    stages {
        stage('Extract Payload Hash') {
            steps {
                script {
                    echo "Git Revision List: ${env.MY_VARIABLE}"
                }

                sh 'cd /var/jenkins_home/workspace | ls -la' 
                sh 'docker build -t docker-frontend .'
                sh "docker tag docker-frontend samavgn02/docker-frontend:${env.MY_VARIABLE}"
                sh "docker login -u ${DOCKERHUB_USERNAME} -p ${DOCKERHUB_PASSWORD}"
                sh "docker push samavgn02/docker-frontend:${env.MY_VARIABLE}"
            }
        }
    }
}
