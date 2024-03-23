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
                // git branch: 'main', credentialsId: 'github-environments', url: 'https://github.com/Avagyan02/docker-frontend-backend-db.git'
                
                
                // sh "docker pull ${DOCKERHUB_USERNAME}/docker-frontend:${env.MY_VARIABLE}"
                // sh 'ls -la'
                // sh "cd /var"
                // sh "ls -la"

                // sh "rm -r docker-frontend-backend-db"
                // sh "git clone https://github.com/Avagyan02/docker-frontend-backend-db.git" 

                // sh "cat "

                sh 'ls -la'

                // sh "cd /var/docker-frontend-backend-db"
                // sh 'ls -la'
                // sh "git checkout main"
                // sh 'ls -la'
                // sh 'cd frontend'
                // sh "rm -r Dockerfile"
                // sh "docker save ${DOCKERHUB_USERNAME}/docker-frontend:${env.MY_VARIABLE} > /var/docker-frontend-backend-db/frontend"
                // sh "git status"
                // sh "git add ."
                // sh "git config --global user.email samvel.avagyan.02@bk.ru"
                // sh "git config --global user.name Avagyan02"
                // sh "git commit -m 'update frontend-jenkins-file'"
                // sh "git branch"
                // sh "git remote add origin https://Avagyan02:1Samvel2002@github.com/Avagyan02/project.git"
                // sh "git push origin main"


                // dir('/root') {
                //     git credentials: GIT_CREDENTIALS, url: 'https://github.com/Avagyan02/docker-frontend-backend-db.git'
                //     sh 'ls -la'
                // }


                // script {
                //     sh 'cd ../'
                //     sh 'ls -la'
                //     sh 'cd docker-frontend-backend-db'
                //     sh 'ls -la'
                //     sh "bash ./docker-frontend-backend-db/docker-compose-file-frontend-build-value-change.sh ${DOCKERHUB_USERNAME}/docker-frontend:${env.MY_VARIABLE}"
                //     sh "bash ./docker-frontend-backend-db/docker-compose-file-version-change.sh"
                // }


                withCredentials([
                    gitUsernamePassword(credentialsId: 'github-environments', gitToolName: 'Default')
                ]) {
                    if [ -d "/var/jenkins_home/workspace/docker-frontend-backend-db" ]; then
                        rm -rf "docker-frontend-backend-db"
                    fi

                    sh "git clone https://github.com/Avagyan02/docker-frontend-backend-db.git"
                    sh "ls -la"
                    sh "bash ./docker-frontend-backend-db/docker-compose-file-frontend-build-value-change.sh ${DOCKERHUB_USERNAME}/docker-frontend:${env.MY_VARIABLE}"
                    sh "bash ./docker-frontend-backend-db/docker-compose-file-version-change.sh"                    
                    sh "git add ."
                    sh "git commit -m 'update front docker file"
                    sh "git push origin"
                }
            }   
        }
    }
}
