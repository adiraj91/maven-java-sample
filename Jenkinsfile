pipeline {
    agent any

    stages {
        stage('clean') {
            steps {
                powershell 'mvn clean'
            }
        }
        stage('build') {
            steps {
                powershell 'mvn compile'
            }
        }
        stage('test') {
            steps {
                powershell 'mvn test'
            }
        }
        stage('package') {
            steps {
                powershell 'mvn package'
            }
            post {
              always {
                // One or more steps need to be included within each condition's block.
                junit 'target/surefire-reports/*.xml'
              }
              success {
                // One or more steps need to be included within each condition's block.
                archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
              }
            }
        }
        stage('Deploy') {
            steps {
                withEnv(['JENKINS_NODE_COOKIE=dontKillMe']){
                    sh 'nohup java -jar -DServer.port=8081 target/spring-petclinic-2.3.1.BUILD-SNAPSHOT.jar &'
                }
            }
        }
    }
}
