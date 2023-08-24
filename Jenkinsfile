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
        
        stage('SonarQube Analysis') {
            steps {
                script {
                    def mvnHome = tool name: 'Maven', type: 'hudson.tasks.Maven$MavenInstallation'
                    def pomFile = 'JavaWebApp/pom.xml'
                    def sonarKey = credentials('PROJECT_KEY')
                    def sonarHostUrl = credentials('HOST_URL')
                    def sonarLogin = credentials('SONAR_LOGIN')
                    
                    if (mvnHome) {
                        sh "${mvnHome}/bin/mvn -f ${pomFile} sonar:sonar \
                            -Dsonar.projectKey=${sonarKey} \
                            -Dsonar.host.url=${sonarHostUrl} \
                            -Dsonar.login=${sonarLogin}"
                    } else {
                        sh "mvn -f ${pomFile} sonar:sonar \
                            -Dsonar.projectKey=${sonarKey} \
                            -Dsonar.host.url=${sonarHostUrl} \
                            -Dsonar.login=${sonarLogin}"
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
