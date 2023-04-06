pipeline {
    agent any

    environment{
        DOCKERHUB_USERNAME = "vishalbasani"
        APP_NAME = "devops-integration"
        //IMAGE_TAG = "${BUILD_NUMBER}"
        IMAGE_NAME = "${DOCKERHUB_USERNAME}" + "/" + "${APP_NAME}"
    }

    stages{
        stage('Clone repository'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Vishal-1705/devops-automation-manifest']]])
            }
        }
        stage('Update GIT') {
            steps{
                script {
                    catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                        withCredentials([usernamePassword(credentialsId: 'github-credentials', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                            //def encodedPassword = URLEncoder.encode("$GIT_PASSWORD",'UTF-8')
                            sh "git config user.email vishalreddy304@gmail.com"
                            sh "git config user.name Vishal"
                            //sh "git switch master"
                            sh "cat deploymentservice.yaml"
                            sh "sed -i 's/${APP_NAME}.*/${APP_NAME}:${DOCKERTAG}/g' deploymentservice.yaml"
                            sh "cat deploymentservice.yaml"
                            sh "git add ."
                            sh "git commit -m 'Done by Jenkins Job changemanifest: ${env.BUILD_NUMBER}'"
                            sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/devops-automation-manifest.git HEAD:main"
                        }
                    }
                }
            }
        }
    }
}