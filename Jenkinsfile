pipeline {
    agent {
        docker {
            image 'node:10-alpine'
            args '-p 3000:3000'
        }
    }
    environment {
        CI = 'true'
	HOME = '.'
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
stage('Code Quality') {
                   steps {
                       script {
                          def scannerHome = tool 'SonarQubeScanner';
                          withSonarQubeEnv("sonarqube") {
                          sh "${tool("SonarQubeScanner")}/bin/sonar-scanner -e -Dsonar.host.url=${SONAR_HOST}"
                                       }
                               }
                           }
                        }


        stage('Sonarqube') {
    environment {
        scannerHome = tool 'SonarQubeScanner'
    }    
    steps {
        withSonarQubeEnv('sonarqube') {
           sh  '/var/lib/jenkins/tools/hudson.plugins.sonar.SonarRunnerInstallation/SonarQubeScanner/bin/sonar-scanner'
        }        
        timeout(time: 10, unit: 'MINUTES') {
            waitForQualityGate abortPipeline: true
        }
    }
}
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)?'
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}
