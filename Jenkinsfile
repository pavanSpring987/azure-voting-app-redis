pipeline { 

    agent any

    stages { 

        stage('Hello') { 

            steps { 
                echo 'hello world'
            }
            
        }

        stage('Verify Branch') { 

            steps { 
                sh 'echo hello-shell'
                echo "$GIT_BRANCH"
            }
            
        }

        stage('Docker Container') { 

            steps { 
                sh(script: """
                pwd
                ls -lt
                """)
                
            }
            
        }
    }
}