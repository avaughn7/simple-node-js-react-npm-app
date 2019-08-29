pipeline {
    agent {
        docker {
            image 'node:6-alpine' 
            args '-p 3000:3000' 
        }
    }
    environment {
        CI = 'true'
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
        stage('Deliver') {
          steps {
              sh './jenkins/scripts/deliver.sh'
              input {
                  id: 'devdeployapproval',
                  message: 'Finished using the web site??? (Click "Proceed" to continue)',
                  submitter "Bob,Tom",
                  parameters {
                    string(name:'username', defaultValue: 'user', description: 'Username of the user pressing Ok')
                  }
              }
              echo "User: ${username} said Ok."
              sh './jenkins/scripts/kill.sh'
          }
        }
        // stage('Deliver') {
        //     steps {
        //         sh './jenkins/scripts/deliver.sh'
        //         input message: 'Finished using the web site? (Click "Proceed" to continue)'
        //         sh './jenkins/scripts/kill.sh'
        //     }
        // }
    }
}