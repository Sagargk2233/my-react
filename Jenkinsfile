pipeline {
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  stages {
    stage('Checkout') {
      steps {
        echo 'Checking out the code...'
        checkout scm
      }
    }
    stage('Install Dependencies') {
      steps {
        echo 'Installing dependencies...'
        bat 'npm install'
      }
    }
    stage('Build') {
      steps {
        echo 'Building the application...'
        bat 'npm run build'
      }
    }
     stage('Deploy') {
            steps {
                script {
                    def buildDir = 'build'
                    def deployDir = 'C:\\My PC\\MCA-SEM-4\\NewReactProject\\Deployments\\portfolio'
                    bat "if exist ${deployDir}\\* del /Q ${deployDir}\\*"
                    bat "xcopy \"${buildDir}\\*\" \"${deployDir}\" /E /I /Y /Q"
                }
            }
        }
    stage('Serve') {
      steps {
        echo 'Deploying to Local...'
        script {
            echo 'Serving the application locally...'
            powershell 'Start-Process powershell -ArgumentList "npm run serve"'
        }
      }
    }
    stage('Open in Chrome') {
      steps {
        echo 'Opening the application in Chrome...'
        script {
          powershell 'Start-Process chrome http://localhost:8070'
        }
      }
    }
  }
}