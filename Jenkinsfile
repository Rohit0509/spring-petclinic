pipeline {
    agent{ label : 'linux' }
    stages {
        stage('Checkout'){
            steps {
                git 'git@github.com:Rohit0509/spring-petclinic.git'
            }
        }
        stage('Build'){
            steps {
                sh 'mvn clean package'
                junit '**/target/test-reports/TEST-*.xml'
                archiveArtifacts artifacts : 'target/*.jar'
            }
        }
        stage('Deploy'){
            steps {
                input 'Do you approve the deployment?'
                sh 'cp target/*.jar /var/apps/'
                sh 'mv *.jar build.jar'
                sh 'java -jar /var/apps/build.jar &'
            }
        }
    }
}
