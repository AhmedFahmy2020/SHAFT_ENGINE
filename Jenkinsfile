pipeline {
  agent any
  stages {
    stage('EnvironmentSetup') {
      agent {
        dockerfile {
          filename 'https://raw.githubusercontent.com/carlossg/docker-maven/26ba49149787c85b9c51222b47c00879b2a0afde/openjdk-14/Dockerfile'
        }

      }
      steps {
        sh '''mvn test -DexecutionAddress="localhost:4444" -DtargetOperatingSystem="Linux-64" -DmaximumPerformanceMode="true" -DtargetBrowserName="GoogleChrome" -Dtest="!%regex[.*checksum.*], !%regex[.*cucumber.*], !%regex[.*sikulix.*], !%regex[.*imageComparison.*], !%regex[.*FileActions.*], !%regex[.*TerminalActions.*], !%regex[.*localShell.*], !%regex[.*fullPageScreenshotWithHeader.*], !%regex[.*dbConnection.*], !%regex[.*Appium.*]"'''
      }
    }

  }
}
