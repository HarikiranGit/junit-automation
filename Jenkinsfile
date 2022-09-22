pipeline {

    agent any 
    stages {
        stage('Build'){
            steps{
                
               
                
                sh 'mkdir lib'
                sh 'cd lib'
                sh  curl -H 'https://repo1.maven.org/maven2/org/junit/platform/junit-platform-console-standalone/1.7.0/junit-platform-console-standalone-1.7.0-all.jar'
                sh 'cd src ; javac -cp "../lib/junit-platform-console-standalone-1.7.0-all.jar" CarTest.java Car.java App.java'
            }
        }

        stage('Test'){
            steps{
                sh 'cd src/ ; java -jar ../lib/junit-platform-console-standalone-1.7.0-all.jar -cp "." --select-class CarTest --reports-dir="reports"'
                junit 'src/reports/*-jupiter.xml'
            }
        }
        
         

        stage('Deploy'){
            steps{
                sh 'cd src/ ; java App' 
            }
        }
    }

 post {
    always {
        junit(
          allowEmptyResults: true, 
          testResults:  'src/reports/*-jupiter.xml'
        )
        recordIssues(
          enabledForFailure: true, aggregatingResults: true, 
          tools: [java(), checkStyle(pattern: 'src/reports/TEST-junit-vintage.xml', reportEncoding: 'UTF-8')]
        )
    }
  }   
    
}
