pipeline {
    agent any
    
    environment {
        MY_VARIABLE = sh(returnStdout: true, script: 'git rev-parse --short=7 HEAD').trim()
    }

    triggers {
        GenericTrigger(
            token: 'docker-frontend-token' 
        )
    }
    
    stages {
        stage('Push Stage') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'jenkins-environments', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        echo "Username: ${env.USERNAME}"
                        echo "Password: ${env.PASSWORD}"

                        echo "Username: ${env.USERNAME}"
                        echo "Username: 1"

                        sh 'cd /var/jenkins_home/workspace | ls -la' 
                        sh 'docker build -t docker-frontend .'
                        sh "docker tag docker-frontend samavgn02/docker-frontend:${env.MY_VARIABLE}"
                        sh "docker login -u ${env.USERNAME} -p ${env.PASSWORD}"
                        sh "docker push samavgn02/docker-frontend:${env.MY_VARIABLE}" 
                    }
                }
            }
        }

        stage('Pull and Push Stage') {
            steps {
                sh "docker pull docker-frontend:${env.MY_VARIABLE}"
                sh "cd /var/jenkins_home/workspace"
                sh "git clone https://github.com/Avagyan02/docker-frontend-backend-db.git" 
                sh "cd docker-frontend-backend-db"
                sh "rm -r Dockerfile"
                sh "docker save docker-frontend:${env.MY_VARIABLE} > /var/jenkins_home/workspace/docker-frontend-backend-db"
                sh "git status"
                sh "git add ."
                sh "git commit -m 'update frontend-jenkins-file'"
                sh "git push origin"
            }
        }
    }
}
