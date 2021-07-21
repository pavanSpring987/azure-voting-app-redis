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
                echo "$GIT_BRANCH"
            }
            
        }
    }
}