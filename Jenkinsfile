pipeline{
    environment {
        registry = "bvnsaichandu/sampledocker_git"
        registryCredential = '234a8511-2c6b-4c5f-8d8b-57ac8bdf5c02'
        dockerImage = ''
    }
    agent any
       stages{
        stage("git clone"){
            steps{
                git 'https://github.com/bvnsaichandu/sampleDocker_git.git'
            }
        }
        stage("build image"){
            steps{
                script{
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        stage("Push to docker hub"){
            steps{
                script{
                    docker.withRegistry( '', registryCredential ){
                        dockerImage.push()
                    }
                }
            }
        }
    }    
    post("slack notification"){
        success{
            slackSend (color: '#00FF00', message: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
        }
        failure{
                slackSend (color: '#FF0000', message: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
        }         
    }
}

