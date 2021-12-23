pipeline {
    agent any
    tools {
        maven "maven 3.8.4"
    }
    stages {
        stage('scm') {
            steps {
              git branch: 'dev', credentialsId: 'git-credentials', url: 'https://github.com/valirocks143/maven-calculationApp.git'
            }
        }
    
        stage('Clean') {
             steps {
                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true clean"
                }
        }
  
        stage('Package') {
             steps {
                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true package"
             }
        }
        stage('Artifact') {
            steps {
                // maven artifacts
               archiveArtifacts 'target/*.war'
              // archiveArtifacts artifacts: 'target/*.war', followSymlinks: false
            }
        }
        stage('connection to tomcat server') {
            steps {
            withCredentials([sshUserPrivateKey(credentialsId: 'linex_credential', keyFileVariable: 'key veriable', passphraseVariable: 'tomcat veriable', usernameVariable: 'vannur key')]) { 
            echo "sucessfully connected to server"    
                }
            }
        }
        stage('deply') {
            steps {
            withCredentials([usernameColonPassword(credentialsId: 'tomcatcredential', variable: 'tomcatservercredential')]) {
           sh "curl -v -u ${tomcatservercredential} -T /var/lib/jenkins/workspace/mvn_calculater/target/CalculationMavenApp.war 'http://ec2-3-142-173-132.us-east-2.compute.amazonaws.com:8080/manager/text/deploy?path=/mvn_calculater&update=true'"
                }
            }
        }
        
    }
}
