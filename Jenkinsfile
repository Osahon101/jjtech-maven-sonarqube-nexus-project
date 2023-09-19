def COLOR_MAP = [
    'SUCCESS': 'good',
    'FAILURE': 'danger',
]

pipeline {
    agent any
    
//     stages {
//         stage('Build') {
//             steps {
//                 script {
//                     def mvnHome = tool name: 'Maven', type: 'hudson.tasks.Maven$MavenInstallation'
//                     def pomFile = 'JavaWebApp/pom.xml'
                    
//                     if (mvnHome) {
//                         sh "${mvnHome}/bin/mvn -f ${pomFile} clean package"
//                     } else {
//                         sh "mvn -f ${pomFile} clean package"
//                     }
//                 }
//             }
//         }
        
//         stage('SonarQube Analysis') {
//             steps {
//                 script {
//                     def mvnHome = tool name: 'Maven', type: 'hudson.tasks.Maven$MavenInstallation'
//                     def pomFile = 'JavaWebApp/pom.xml'
//                     def sonarKey = credentials('PROJECT_KEY')
//                     def sonarHostUrl = credentials('HOST_URL')
//                     def sonarLogin = credentials('SONAR_LOGIN')
                    
//                     if (mvnHome) {
//                         sh script: "${mvnHome}/bin/mvn -f ${pomFile} sonar:sonar -Dsonar.projectKey=${sonarKey} -Dsonar.host.url=${sonarHostUrl} -Dsonar.login=${sonarLogin}", 
//                           returnStatus: true
//                     } else {
//                         sh script: "mvn -f ${pomFile} sonar:sonar -Dsonar.projectKey=${sonarKey} -Dsonar.host.url=${sonarHostUrl} -Dsonar.login=${sonarLogin}", 
//                           returnStatus: true
//                     }
//                 }
//             }
//         }
//     }
    
//     post {
//         always {
//             archiveArtifacts artifacts: '**/target/*.jar', allowEmptyArchive: true
//         }
//     }
// }

// pipeline {
    
// //      agent {
// //       node {
// //     label 'jenkins-agent1'
// //   }
// // }
//     agent any
//     // agent {
//     //     docker { image 'maven:3.8.6-openjdk-11' }
//     // }

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
                 git branch: 'main', url: 'https://github.com/Osahon101/jjtech-maven-sonarqube-nexus-project.git'
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
          stage('approval') {
            steps {
                input "Please review the test and click 'Proceed' to apply it"
            }
        }

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
  -Dsonar.projectKey=jenkins-maven-project \
  -Dsonar.host.url=http://172.31.13.30:9000 \
  -Dsonar.login=$SONAR_TOKEN
                     """
                    }
                // }
            }
        }
    }

        //           stage('Upload artifact to Nexus') {
        //     steps {
        //         withCredentials([usernamePassword(credentialsId: 'nexus-credentials', passwordVariable: 'PASSWORD', usernameVariable: 'USER_NAME')]) {
        //         sh "sed -i \"s/.*<username><\\/username>/<username>$USER_NAME<\\/username>/g\" ${WORKSPACE}/settings.xml"
        //         sh "sed -i \"s/.*<password><\\/password>/<password>$PASSWORD<\\/password>/g\" ${WORKSPACE}/settings.xml"
        //         sh 'sudo cp ${WORKSPACE}/settings.xml /var/lib/jenkins/.m2'
        //         dir('JavaWebApp/') {                
        //         sh 'mvn clean deploy -DskipTests'
        //         }
        //       }
               
        //     }
        // }

          stage('Upload artifact to Nexus') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'nexus-credentials', passwordVariable: 'PASSWORD', usernameVariable: 'USER_NAME')]) {
                sh "sed -i \"s/.*<username><\\/username>/<username>$USER_NAME<\\/username>/g\" ${WORKSPACE}/settings.xml"
                sh "sed -i \"s/.*<password><\\/password>/<password>$PASSWORD<\\/password>/g\" ${WORKSPACE}/settings.xml"
                sh "sed -i 's|http://172.31.9.23:8081/repository/maven-snapshots/|http://172.31.9.23:8081/repository/maven-snapshots/|g' ${WORKSPACE}/settings.xml"
                sh "sed -i 's|http://172.31.9.23:8081/repository/maven-releases/|http://172.31.9.23:8081/repository/maven-releases/|g' ${WORKSPACE}/settings.xml"
                sh "sed -i 's|<sonar.host.url>http://172.31.13.30:9000</sonar.host.url>|<sonar.host.url>http://172.31.13.30:9000</sonar.host.url>|g' ${WORKSPACE}/settings.xml"
                sh 'sudo cp ${WORKSPACE}/settings.xml /var/lib/jenkins/.m2'
                dir('JavaWebApp/') {
                sh "sed -i 's|http://172.31.9.23:8081/repository/maven-snapshots/|http://172.31.9.23:8081/repository/maven-snapshots/|g' pom.xml"
                sh "sed -i 's|http://1172.31.9.23:8081/repository/maven-releases/|http://172.31.9.23:8081/repository/maven-releases/|g' pom.xml"                
                sh 'mvn clean deploy -DskipTests'
                }
              }
               
            }
        }

    // stage('Deploy to Development Env') {
    //     environment {
    //         HOSTS = 'dev'
    //     }
    //     steps {
    //         withCredentials([usernamePassword(credentialsId: 'ansible-deploy-server-credentials', passwordVariable: 'PASSWORD', usernameVariable: 'USER_NAME')]) {
    //             sh "ansible-playbook -i ${WORKSPACE}/ansible-setup/aws_ec2.yaml ${WORKSPACE}/deploy.yaml --extra-vars \"ansible_user=$USER_NAME ansible_password=$PASSWORD hosts=tag_Environment_$HOSTS workspace_path=$WORKSPACE\""
    //         }
    //     }
    // }

    stage('Deploy to Development Env') {
        environment {
            HOSTS = 'dev'
        }
        // steps {
        //     withCredentials([usernamePassword(credentialsId: 'ansible-deploy-server-credentials', passwordVariable: 'PASSWORD', usernameVariable: 'USER_NAME')]) {
        //         sh "ansible-playbook ${WORKSPACE}/deploy.yaml --extra-vars \"hosts=$HOSTS workspace_path=$WORKSPACE\""
        //     }
        // }
        steps {
            withCredentials([usernamePassword(credentialsId: 'ansible-deploy-server-credentials', passwordVariable: 'PASSWORD', usernameVariable: 'USER_NAME')]) {
                sh "ansible-playbook -i ${WORKSPACE}/ansible-setup/aws_ec2.yaml ${WORKSPACE}/deploy.yaml --extra-vars \"ansible_user=$USER_NAME ansible_password=$PASSWORD hosts=tag_Environment_$HOSTS workspace_path=$WORKSPACE\""
            }
        }
        
        
    }
    stage('Deploy to Staging Env') {
        environment {
            HOSTS = 'stage'
        }
        steps {
            withCredentials([usernamePassword(credentialsId: 'ansible-deploy-server-credentials', passwordVariable: 'PASSWORD', usernameVariable: 'USER_NAME')]) {
                sh "ansible-playbook -i ${WORKSPACE}/ansible-setup/aws_ec2.yaml ${WORKSPACE}/deploy.yaml --extra-vars \"ansible_user=$USER_NAME ansible_password=$PASSWORD hosts=tag_Environment_$HOSTS workspace_path=$WORKSPACE\""
            }
        }
    }
    stage('Quality Assurance Approval') {
        steps {
            input('Do you want to proceed?')
        }
    }
    stage('Deploy to Production Env') {
        environment {
            HOSTS = 'prod'
        }
        steps {
            withCredentials([usernamePassword(credentialsId: 'ansible-deploy-server-credentials', passwordVariable: 'PASSWORD', usernameVariable: 'USER_NAME')]) {
                sh "ansible-playbook -i ${WORKSPACE}/ansible-setup/aws_ec2.yaml ${WORKSPACE}/deploy.yaml --extra-vars \"ansible_user=$USER_NAME ansible_password=$PASSWORD hosts=tag_Environment_$HOSTS workspace_path=$WORKSPACE\""
            }
         }
      }
  }
}

  post {
    always {
        echo 'Slack Notifications.'
        slackSend channel: '#tower-badge', //update and provide your channel name
        color: COLOR_MAP[currentBuild.currentResult],
        message: "*${currentBuild.currentResult}:* Job Name '${env.JOB_NAME}' build ${env.BUILD_NUMBER} \n Build Timestamp: ${env.BUILD_TIMESTAMP} \n Project Workspace: ${env.WORKSPACE} \n More info at: ${env.BUILD_URL}"
    }
}



