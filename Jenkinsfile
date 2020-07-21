pipeline {
  agent any
  stages {
    stage('Setup Selenium Grid') {
      steps {
        sh 'docker-compose -f docker-compose_native.yml up --scale chrome=4 --remove-orphans -d'
      }
    }

    stage('Run Tests') {
      agent {
        docker {
          image 'maven:3.6.3-openjdk-14'
        }
      }
      steps {
        sh 'mvn test -DexecutionAddress="localhost:4444" -DtargetOperatingSystem="Linux-64" -DmaximumPerformanceMode="true" -DtargetBrowserName="GoogleChrome" -Dtest="!%regex[.*checksum.*], !%regex[.*cucumber.*], !%regex[.*sikulix.*], !%regex[.*imageComparison.*], !%regex[.*FileActions.*], !%regex[.*TerminalActions.*], !%regex[.*localShell.*], !%regex[.*fullPageScreenshotWithHeader.*], !%regex[.*dbConnection.*], !%regex[.*Appium.*]"'
      }
    }

    stage('Teardown') {
      steps {
        sh 'docker-compose -f docker-compose_native.yml down --remove-orphans'
      }
    }

  }
}
