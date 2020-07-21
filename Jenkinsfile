pipeline {
  agent any
  stages {
    stage('Setup Selenium Grid') {
      agent any
      steps {
        bat(script: 'docker-compose -f docker-compose_native.yml up --scale chrome=4 --remove-orphans -d', returnStdout: true, returnStatus: true)
      }
    }

    stage('Run Tests') {
      agent {
        docker {
          image 'maven:3.6.3-openjdk-14'
        }

      }
      environment {
        MSYS_NO_PATHCONV = '1'
      }
      steps {
        powershell(script: 'mvn test -DexecutionAddress="localhost:4444" -DtargetOperatingSystem="Linux-64" -DmaximumPerformanceMode="true" -DtargetBrowserName="GoogleChrome" -Dtest="!%regex[.*checksum.*], !%regex[.*cucumber.*], !%regex[.*sikulix.*], !%regex[.*imageComparison.*], !%regex[.*FileActions.*], !%regex[.*TerminalActions.*], !%regex[.*localShell.*], !%regex[.*fullPageScreenshotWithHeader.*], !%regex[.*dbConnection.*], !%regex[.*Appium.*]"', returnStatus: true, returnStdout: true)
      }
    }

  }
}