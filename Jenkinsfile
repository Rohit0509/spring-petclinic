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
