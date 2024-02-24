pipeline{
    agent any
    stages{
        stage("StaticCodeAnalysis"){
            agent {
                docker {
                    image 'openjdk:11'
                }
            }
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                        sh 'chmod +x gradlew'
                        sh './gradlew sonarqube'
                  }
                }
            }
        }

        stage("CheckSonarQulaityGate"){
            steps {
                script {
                    timeout(time: 1, unit: "Hours"){
                        def qg = waitForQualityGate()
                    if (qg.status != 'OK') {
                        error "Pipeline is aborted due to reason - ${qg.status}"
                    }
                    else{
                        print "pipeline succesfull"
                    }
                  }
                }
            }
        }
    }
}