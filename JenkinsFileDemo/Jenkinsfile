pipeline{
    agent any
    
    parameters{
        string(
            name:'APP_PORT',
            defaultValue:'3000',
            description:'server port'
        )
    }

    environment{
            
            IMAGE_NAME='Jenkins-demo-app'
        }
      stages{
            stage('Checkout'){
                steps{
                    echo 'Checking out Source code From Git Repo'
                    checkout scm
                }
            }
            stage('Check Docker'){
                steps{
                    bat 'docker --version'
                }
            }
            stage('Dependencies'){
                steps{
                    bat 'npm install'
                }
            }
            stage('Test APP'){
                steps{
                    bat 'npm test'
                }
            }
            stage('Build'){
                steps{
                    bat 'docker build -t %IMAGE_NAME%:%BUILD_NUMBER% .'
                }
            }
            stage('RUN Container'){
                steps{
                    bat '''
                    docker run -d --name node-app-%BUILD_NUMBER% -p %APP_PORT%:3000 %IMAGE_NAME%:%BUILD_NUMBER%
                    '''
                }
            }
            stage('verify'){
                steps{
                    bat '''
                      echo APP Deployed Successfully
                      echo Open http://localhost:%APP_PORT%
                      docker ps
                      '''
                }
            }

        }
}
