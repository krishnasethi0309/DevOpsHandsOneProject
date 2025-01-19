pipeline {
    agent any //We can run the job in any nodes wither master or in slave node
    
    tools {
        maven 'Maven3' // Configure in tool section which Maven version need to use
    }

    stages {
        stage ('Git Clone’) {//Stage 1
            steps {
                    git 'https://github.com/jenkins-docs/simple-java-maven-app.git'    
            }
        }
        
         stage ('Code Build') {//Stage 2
            steps {
                    bat 'mvn clean package'   
            }
        }
        
        stage ('Code Test') { //Stage 3
            steps {
                    bat 'mvn test'   
            }            
            post {
                    always {
                        junit '**/target/surefire-reports/TEST-*.xml'
                    }
            }
        }
        
        stage('Deploy’) {//Stage 4
                steps
            {
                 //Locally transfer the Jar Files
                 echo 'Deploy the application'
            }
        }
    }
    
    post { //Post job execution
            failure { //On Failure
                echo 'Failure!'
                mail to: 'kraja9841@gmail.com',
                subject: "FAILED: ${env.JOB_NAME} - BUILD #${env.BUILD_NUMBER}",
                body: "This is mail Notification - JOB ${JOB_NAME} - ${BUILD_URL} Failed"
            }            
            success { //On Success
                echo 'Build and Test Stages Successfull'
                archiveArtifacts artifacts: '**/target/*.jar', onlyIfSuccessful: true
                
                mail to: 'kraja9841@gmail.com',
                subject: "Passed: ${env.JOB_NAME} - BUILD #${env.BUILD_NUMBER}",
                body: "This is mail Notification - JOB ${JOB_NAME} - ${BUILD_URL} Passed"
            }
    }
}
