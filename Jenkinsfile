pipeline {
    agent any
    stages {
        
        stage('Build'){
            steps {
                sh 'mvn clean package'
                
                archiveArtifacts artifacts : 'target/*.jar'
            }
        }
        stage('Deploy'){
            steps {
                input 'Do you approve the deployment?'
                sh 'cp target/*.jar /var/apps/'
                sh 'java -jar /var/apps/*.jar &'
            }
        }
    }
}
