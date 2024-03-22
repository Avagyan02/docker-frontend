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
                        echo "Password: ${env.PASSWORD}"

                        echo "Username: ${env.USERNAME}"
                        echo "Username: 1" 
                    }
                }

                sh 'cd /var/jenkins_home/workspace | ls -la' 
                sh 'docker build -t docker-frontend .'
                sh "docker tag docker-frontend ${DOCKERHUB_USERNAME}/docker-frontend:${env.MY_VARIABLE}"
                sh "docker login -u ${DOCKERHUB_USERNAME} -p ${DOCKERHUB_PASSWORD}"
                sh "docker push ${DOCKERHUB_USERNAME}/docker-frontend:${env.MY_VARIABLE}"
            }
        }

        stage('Pull and Push Stage') {
            steps {
                sh "docker pull ${DOCKERHUB_USERNAME}/docker-frontend:${env.MY_VARIABLE}"
                sh 'ls -la'
                sh "cd /var/jenkins_home/workspace"

                script {
                    // sh "rm -r docker-frontend-backend-db"
                    def devopsProjectPath = '/var/jenkins_home/workspace'
                    // def folder = new File(devopsProjectPath)

                    echo "------- 1 ${fileExists(devopsProjectPath)}"

                    // if (folder.exists() && folder.isDirectory()) {
                    if (fileExists(devopsProjectPath)) {
                        sh "cd docker-frontend-backend-db"
                        sh "git pull origin main"
                    } else {
                        sh "git clone https://github.com/Avagyan02/docker-frontend-backend-db.git" 
                    }
                }

                sh "cd docker-frontend-backend-db"
                sh "git checkout main"
                sh 'ls -la'
                sh "rm -r Dockerfile"
                sh "docker save ${DOCKERHUB_USERNAME}/docker-frontend:${env.MY_VARIABLE} > /var/jenkins_home/workspace/docker-frontend-backend-db"
                sh "git status"
                sh "git add ."
                sh "git config --global user.email samvel.avagyan.02@bk.ru"
                sh "git config --global user.name Avagyan02"
                sh "git commit -m 'update frontend-jenkins-file'"
                sh "git branch"
                sh "git remote add origin https://Avagyan02:1Samvel2002@github.com/Avagyan02/project.git"
                sh "git push origin main"

            }
        }
    }
}
