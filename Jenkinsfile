pipeline {
    agent any
    
    environment {
        MY_VARIABLE = sh(returnStdout: true, script: 'git rev-parse --short=7 HEAD').trim()
        DOCKERHUB_USERNAME = null
        DOCKERHUB_PASSWORD = null
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
                    docker.withRegistry('https://index.docker.io/v1/', 'jenkins-environments') {
                        docker.build("samavgn02/docker-frontend")
                        docker.image("samavgn02/docker-frontend").push(env.MY_VARIABLE)
                    }
                }
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