pipeline {
    agent { label 'ltecomm'}
    stages {
        stage('scm') {
            steps {
                git branch: 'developer', url:'https://github.com/challagcp/qt-gol.git'        
            }
        }
        stage('build') {
            steps {
                withSonarQubeEnv('SONAR-7.1') {
                    sh script: 'mvn clean package sonar:sonar'

                }
                
            }
        }
        
        stage('post build') {
            steps {
                junit 'gameoflife-web/target/surefire-reports/*.xml'
                archiveArtifacts 'gameoflife-web/target/*.war'
            }
        }
        stage("Quality Gate") {
            steps {
              timeout(time: 1, unit: 'HOURS') {
                waitForQualityGate abortPipeline: true, credentialsId: 'SONAR_TOKEN'
              }
            }
        }
    }
}