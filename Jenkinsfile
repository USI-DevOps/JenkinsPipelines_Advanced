pipeline {
  agent any
  stages {
    stage('Get Env') {
      parallel {
        stage('Get Env') {
          steps {
            echo "Get the chromedriver path : ${SELENIUM}"
            pwsh 'Write-Output "Hello Worlds"'
          }
        }

        stage('Script execution') {
          steps {
            script {
              def msg = powershell(returnStdout: true, script: "./script/build.ps1")
              println msg
              VARIABLE=msg
            }

          }
        }

      }
    }

    stage('Deploy') {
      when {
        branch 'master'
      }
      steps {
        input(message: 'Are you sure to deploy', ok: 'Yes')
      }
    }

    stage('Post Build') {
      parallel {
        stage('Generate File') {
          steps {
            writeFile(file: 'deployed.txt', text: "deployed to ${VARIABLE}")
          }
        }

        stage('Archive file') {
          steps {
            archiveArtifacts(artifacts: 'deployed.txt', excludes: '*.log')
          }
        }

      }
    }

  }
  environment {
    SELENIUM = 'C:\\DevOps\\Tools\\Drivers'
    VARIABLE = ''
  }
}
