pipeline{
    agent any
    environment{
        VERSION = "${env.BUILD_ID}"
    }
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

        stage("CheckSonarQualityGate"){
            steps {
                script {
                    timeout(time: 1, unit: 'HOURS') {
                        def qg = waitForQualityGate()
                        if (qg.status != 'OK') {
                            error "Pipeline is aborted due to reason - ${qg.status}"
                        }
                        else {
                            print "Pipeline stage succesful"
                        }
                    }
                }
            }
        }

        stage("DockerBuildandPush") {
            steps {
                withCredentials([string(credentialsId: 'docker_password', variable: 'docker_password')]) {
                    script {
                        sh '''
                        docker build -t 44.202.140.220:8083/springapp:${VERSION} .
                        docker login -u admin -p ${docker_password} 44.202.140.220:8083
                        docker push 44.202.140.220:8083/springapp:${VERSION}
                        docker rmi 44.202.140.220:8083/springapp:${VERSION}
                        '''
                }
                }
            }
        }
    }
}