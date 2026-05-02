pipeline {
    agent any

    stages {
        stage('Cleanup workspace') {
            steps {
                cleanWs()
            }
        }
        
        stage('Checkout from SCM') {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/ekelejames/color-checker-k8s-files.git'
            }
        }
        
        stage('Update GIT') {
            steps {
                script {
                    catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                        withCredentials([usernamePassword(credentialsId: 'githubcred', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                            sh "git config user.email ekelejames16@gmail.com"
                            sh "git config user.name ekelejay"
                            sh "cat deployment.yaml"
                            sh "sed -i 's+ekelejay/color-checker-service:[^[:space:]]*+ekelejay/color-checker-service:${DOCKER_TAG}+g' deployment.yaml"
                            sh "cat deployment.yaml"
                            sh "git add ."
                            sh "git commit -m 'Done by Jenkins Job change manifest: ${env.BUILD_NUMBER}'"
                            sh '''
                                git remote set-url origin https://$GIT_USERNAME:$GIT_PASSWORD@github.com/ekelejames/color-checker-k8s-files.git
                                git push origin HEAD:main
                            '''
                        }
                    }
                }
            }
        }
    }
}