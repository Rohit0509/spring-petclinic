pipeline {
    agent any
    stages {
        
        stage('Test/Quality Gate'){
            steps {
                sh 'mvn clean'
                sh 'mvn install'
                withSonarQubeEnv('sonar-1') { 
                   sh 'mvn sonar:sonar'
                }

                timeout(time: 1, unit: 'HOURS') {
                    script {
                        def qg = waitForQualityGate() 
                        echo "My variable is ${qg.status}"
                        echo " ==>>>  ${qg.analysedAt}"
                        echo "==>>> ${qg.project.url}"
                        if (qg.status != 'OK' ) {
                            error "Pipeline aborted due to quality gate failure: ${qg.status}"
                        }                        
                    }   
                }
            }
        }
        
        stage("Build") { 
            steps {
                sh 'mvn package'
                archiveArtifacts artifacts : 'target/*.jar'    
            }
        }
        
        stage('Deploy'){
            steps {
                sh 'rm -rf /var/apps/*'
                sh 'mv target/*.jar /var/apps/'
                sh 'java -jar /var/apps/*.jar &'
            }
        }
    }

    post {
        always {
            echo 'This will always run'
        }
        success {
            echo 'This will run only if successful'
        }
        failure {
            echo 'This will run only if failed'
        }
        unstable {
            echo 'This will run only if the run was marked as unstable'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed'
            echo 'For example, if the Pipeline was previously failing but is now successful'
        }
    }
}