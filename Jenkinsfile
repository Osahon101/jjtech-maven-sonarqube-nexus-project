pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                script {
                    def mvnHome = tool name: 'Maven', type: 'hudson.tasks.Maven$MavenInstallation'
                    def pomFile = 'JavaWebApp/pom.xml'
                    
                    if (mvnHome) {
                        sh "${mvnHome}/bin/mvn -f ${pomFile} clean package"
                    } else {
                        sh "mvn -f ${pomFile} clean package"
                    }
                }
            }
        }
    }
    
    post {
        always {
            archiveArtifacts artifacts: '**/target/*.jar', allowEmptyArchive: true
        }
    }
}
