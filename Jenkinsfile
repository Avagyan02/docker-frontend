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


                // withCredentials([
                //     gitUsernamePassword(credentialsId: 'github-environments', gitToolName: 'Default')
                // ]) {
                    // script {
                    //     if (fileExists("/var/jenkins_home/workspace/docker-frontend-backend-db")) {
                    //         sh "rm -rf docker-frontend-backend-db"
                    //     }
                    // }

                    // sh '[ -d "/var/jenkins_home/workspace/docker-frontend-backend-db" ] && rm -r "docker-frontend-backend-db"'
                    
                // sh 'find / -type d -name "docker-frontend-backend-db"'
                // sh 'echo 2222'

                // sh 'rm -r /var/jenkins_home/workspace/docker-frontend/docker-frontend-backend-db'
                // sh 'ls -la'
                // sh 'git -C /var/jenkins_home/workspace/docker-frontend-backend-db pull origin master'
                
                // sh 'cd /var/jenkins_home'
                // sh "git clone https://github.com/Avagyan02/docker-frontend-backend-db.git"
                // sh "ls -la"
                // sh "bash ./docker-frontend-backend-db/docker-compose-file-frontend-build-value-change.sh ${DOCKERHUB_USERNAME}/docker-frontend:${env.MY_VARIABLE}"
                // sh "bash ./docker-frontend-backend-db/docker-compose-file-version-change.sh"                    
                // sh "git add ."
                // sh "git commit -m 'update front docker file"
                // sh "git push origin"
                // }


                // sh 'git clone https://github.com/Avagyan02/docker-frontend-backend-db.git || true'
                // sh 'cat ./docker-frontend-backend-db/docker-compose-file-frontend-build-value-change.sh'
                // sh "bash ./docker-frontend-backend-db/docker-compose-file-frontend-build-value-change.sh ${DOCKERHUB_USERNAME}/docker-frontend:${env.MY_VARIABLE}"
                // sh "bash ./docker-frontend-backend-db/docker-compose-file-version-change.sh"                    
                // sh "git add ."
                // sh "git -C ./docker-frontend-backend-db commit -m 'update front docker file"
                // sh "git -C ./docker-frontend-backend-db push origin"

                git(
                    url: "https://github.com/DavoA/TaskDevops.git",
                    branch: "main",
                    changelog: true,
                    poll: true
                )
                
                sh "bash docker-compose-file-frontend-build-value-change.sh ${DOCKERHUB_USERNAME}/docker-frontend:${env.MY_VARIABLE}"
                sh "bash docker-compose-file-version-change.sh"                    

                sh "git add ."
                sh "git commit -m 'changing 2'"
                withCredentials([gitUsernamePassword(credentialsId: 'github-environments', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                    sh "git push -u origin master"
                }
            }   
        }
    }
}