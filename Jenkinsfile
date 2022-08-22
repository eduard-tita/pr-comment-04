pipeline {
    agent any

    tools {
        jdk 'Java 8'
    }

    stages {
        stage('Build') { 
            steps {
                //sh 'mvn -B -DskipTests clean install' 
            }
        }
        stage('IQ Policy Evaluation') {
            steps {                
                nexusPolicyEvaluation failBuildOnNetworkError: false, 
                        iqApplication: selectedApplication('local-iq-app'), 
                        iqStage: 'build', iqScanPatterns: [[scanPattern: '.']]                                
            }
        }                
    }
    
    post {
        always {
            deleteDir()
        }
    }
}
