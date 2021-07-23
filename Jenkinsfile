pipeline { 

    agent any

    stages { 

        stage('Verify Branch') { 

            steps { 
                sh 'echo hello-shell'
                echo "$GIT_BRANCH"
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

      
      
      stage('Stop test app') {
         steps {
            sh(script: """
               docker-compose down
            """)
         }
      }

    }
}