pipeline {
    
    agent any
    
    environment {
        dockerImage =''
        registry = 'vinaybr07-java'
        registryCredential = 'Docker'
    }
        stages {
            
            stage ('checkout') {
                steps {
                    checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/mail2vinaybr/docker-image1.git']]])
                }
            }
            stage ('Build Docker image') {
                steps {
                    script {
                        dockerImage = docker.build registry
                    }
                }
            }
            stage ('Uploading image') {
                steps {
                    script {
                            docker.withRegistry( '', registryCredential ) {
                            dockerImage.push()
                        }
                    }   
                }
            }
            stage('docker stop container') {
                steps {
                    sh 'docker ps -f name=mypythonappContainer -q | xargs --no-run-if-empty docker container stop'
                    sh 'docker container ls -a -fname=mypythonappContainer -q | xargs -r docker container rm'
                }
            }
            stage('Docker Run') {
                steps{
                    script {
                        dockerImage.run("-p 8081:5000 --rm --name mypythonappContainer")
                    }
                }
            }
        }
}
