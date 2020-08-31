pipeline {
    agent any
    stages {
        
        stage('Build'){
            steps {
                sh 'mvn clean'
                sh 'mvn install'
                withSonarQubeEnv('sonar-1') { 
                   sh 'mvn sonar:sonar'
                }
                sh 'mvn package'
                archiveArtifacts artifacts : 'target/*.jar'
            }
        }
        stage('Quality Gate'){
              steps {
                  timeout (time : 1, unit: 'HOURS') {
                       def qg = waitForQualityGate()
                        sh "echo ${qg}"
                        if(qg.status != 'OK'){
                            error "Pipeline aborted due to quality gate failure ${qg.status}"
                         }
                        sh "echo 'out'" 
                  }
              }      
        }
        stage('Deploy'){
            steps {
                //input 'Do you approve the deployment?'
                sh 'rm -rf /var/apps/*'
                sh 'cp target/*.jar /var/apps/'
                sh 'java -jar /var/apps/*.jar &'
            }
        }
    }
}
