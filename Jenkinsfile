pipeline{
	agent none
	stages {

	 stage('Sonarqube') {
           agent {
                label 'master'
            }
            steps {
                sh('printenv | sort')
                sh "/opt/sonar-scanner/bin/sonar-scanner -e '-Dsonar.host.url=http://192.168.148.151:9090'"
            }
        }

      stage('Build'){
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
       steps {
                sh 'npm install'
            }
      }

      stage('Test') {
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
        steps {
                sh './jenkins/scripts/test.sh'
            }
        }

       stage('Deliver') {
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
        steps {
                sh './jenkins/scripts/deliver.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)?'
                sh './jenkins/scripts/kill.sh'
            }
        }

    }
}
