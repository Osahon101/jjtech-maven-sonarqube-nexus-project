pipeline {
    
//      agent {
//       node {
//     label 'jenkins-agent1'
//   }
// }
    agent any
    // agent {
    //     docker { image 'maven:3.8.6-openjdk-11' }
    // }

    environment {
        WORKSPACE = "${env.WORKSPACE}"
    }
    
    tools {
      maven 'maven'
    }

    stages {
        stage('git checkout') {
            steps {
                // Get some code from a GitHub repository
                 git branch: 'main', url: 'https://github.com/anselmenumbisia/jjtech-maven-sonarqube-nexus-prometheus-project.git'
            }

            }
        stage('test') {
            steps {
                dir('JavaWebApp/') {
                    echo 'performing mvn test'
                    sh 'mvn clean test'
                    
                }
                
            }

            }
        //   stage('approval') {
        //     steps {
        //         input "Please review the test and click 'Proceed' to apply it"
        //     }
        // }

        stage('build') {
            steps {
                dir('JavaWebApp/') {
                    echo 'performing mvn build'
                    sh 'mvn clean package'
                    
                }
                    
                
            }

            post {
                success {
                    echo 'archiving....'
                    archiveArtifacts artifacts: '**/*.war', followSymlinks: false
                }
            }
        }
        
    //     stage('SonarQube scanning') {
    //         steps {
    //             dir('JavaWebApp/') {
    //             withSonarQubeEnv('SonarQube') {
    //                 withCredentials([string(credentialsId: 'sonarqube-token', variable: 'SONAR_TOKEN')]) {
    //                     sh """
    //                 mvn sonar:sonar \
    //                   -Dsonar.projectKey=tower-project1 \
    //                   -Dsonar.host.url=http://172.31.80.37:9000 \
    //                   -Dsonar.login=942c2c64b96dd2370ac02224ffc2e8e9365aad77
    //                 """
    //                 }
    //             }
    //         }
    //     }
    // }

    //     stage('Quality Gate') {
    //         steps {
    //             waitForQualityGate abortPipeline: true
    //         }
    //     }

            stage('SonarQube scanning') {
            steps {
                dir('JavaWebApp/') {
                // withSonarQubeEnv('SonarQube') {
                    withCredentials([string(credentialsId: 'sonarqube-token', variable: 'SONAR_TOKEN')]) {
                        sh """
                    mvn sonar:sonar \
                    -Dsonar.projectKey=test-project \
                    -Dsonar.host.url=http://54.208.116.33:9000 \
                    -Dsonar.login=ddb982046ec0f3207e073fe8bde69411b6c58c46
                    """
                    }
                }
            }
        }
    }

    }
}
