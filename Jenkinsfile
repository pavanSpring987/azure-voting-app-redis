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
                   usernamePassword(credentials: 'server-credentials', usernameVariable: USER, passwordVariable: PWD )
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
            echo "${SERVER_CREDENTIALS}"
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

      // stage('Run Tests') {
      //    steps {
      //       script { 
      //          docker.image('python') {
      //             sh "python ./tests/test_sample.py"
      //          }
      //       }
            
      //    }
      // }

      
      
      stage('Stop test app') {
         steps {
            sh(script: """
               docker-compose down
            """)
         }
      }

    }
}