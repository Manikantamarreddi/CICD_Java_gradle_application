pipeline{
    agent any
    stages{
        stage("SonarQ!ulaityCheck"){
            agent {
                docker {
                    image 'openjdk:11'
                }
            }
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                        sh 'chmod +x gradleW'
                        sh './gradlew sonarqube'
                  }
                }
            }
        }
    }
}