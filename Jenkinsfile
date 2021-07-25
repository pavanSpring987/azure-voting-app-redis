pipeline { 

    agent any

   //  environment {
   //     SERVER_CREDENTIALS = credentials('server-credentials')
   //  }

    stages { 

        stage('Verify Branch') { 

            steps { 
                sh 'echo hello-shell'
                echo "$GIT_BRANCH"
                withCredentials([
                   usernamePassword(credentialsId: 'server-credentials', usernameVariable: 'USER', passwordVariable: 'PWD' )
                ]) { 
                   echo "${USER} ${PWD}"
                }
            }
            
        }

        stage('Docker Container') { 

            steps { 
                sh(script: """
                cd azure-vote/
                docker images -a
                docker image build -t jenkins-pipeline .
                docker images -a
                """)
                
            }
            
        }

      stage('Start test app') {
         steps {
            // sh(script: """
            //    docker-compose up -d
            // """)
            
            sh 'docker-compose up -d && chmod +x ./scripts/test_container.sh && ./scripts/test_container.sh'
            
         }
         post {
            success {
               echo "App started successfully :)"
            }
            failure {
               echo "App failed to start :("
            }
         }
      }

      stage('Run Tests') {
          agent {
                docker {
                    image 'qnib/pytest'
                }
          }
         steps {
            script { 
              
                  
                  sh "pytest ./tests/test_sample.py"
                  archiveArtifacts "./testResults26-july2021.txt'
             
            }
            
         }
      }

      
      
      stage('Stop test app') {
         steps {
            sh(script: """
               docker-compose down
            """)
         }
      }

    }
}