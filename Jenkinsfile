pipeline {
    agent any
    
    environment {
        MY_VARIABLE = sh(returnStdout: true, script: 'git rev-parse --short=7 HEAD').trim()
        DOCKERHUB_USERNAME = null
        DOCKERHUB_PASSWORD = null
        GIT_CREDENTIALS = credentials('github-environments')
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
                        DOCKERHUB_USERNAME = env.USERNAME
                        DOCKERHUB_PASSWORD = env.PASSWORD
                    }
                }

                sh 'cd /var/jenkins_home/workspace'
                sh 'docker build -t docker-frontend .'
                sh "docker tag docker-frontend ${DOCKERHUB_USERNAME}/docker-frontend:${env.MY_VARIABLE}"
                sh "docker login -u ${DOCKERHUB_USERNAME} -p ${DOCKERHUB_PASSWORD}"
                sh "docker push ${DOCKERHUB_USERNAME}/docker-frontend:${env.MY_VARIABLE}"
            }
        }

        stage('Pull and Push Stage') {
            steps {
                git(
                    url: "https://github.com/Avagyan02/docker-frontend-backend-db.git",
                    branch: "master",
                    changelog: true,
                    credentialsId: 'github-environments',
                    poll: true
                )
                
                sh "bash docker-compose-file-frontend-build-value-change.sh ${DOCKERHUB_USERNAME}/docker-frontend:${env.MY_VARIABLE}"
                sh "bash docker-compose-file-version-change.sh"                    

                sh "git add ."
                sh "git commit -m 'update front jenkins file'"
                withCredentials([gitUsernamePassword(credentialsId: 'github-environments', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                    sh "git push -u origin master"
                }
            }   
        }
    }
}