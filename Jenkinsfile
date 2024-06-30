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
            powershell '''
          # Start the server in the background
          Start-Process -FilePath "cmd.exe" -ArgumentList "/c npm run serve" -PassThru | Select-Object -ExpandProperty Id > server.pid
          '''
          sleep(time: 2, unit: 'MINUTES')
        }
      }
    }
     stage('Terminate Server') {
      steps {
        echo 'Terminating the server...'
        script {
          powershell '''
          # Read the server PID and kill the process
          $pid = Get-Content server.pid
          Stop-Process -Id $pid -Force
          Remove-Item server.pid
          '''
        }
      }
    }
    stage('Open in Chrome') {
      steps {
        echo 'Opening the application in Chrome...'
        script {
          bat 'start chrome http://localhost:8070'
        }
      }
    }
  }
}