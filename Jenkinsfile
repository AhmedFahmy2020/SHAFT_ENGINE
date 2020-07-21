pipeline {
  agent any
  stages {
    stage('Setup Selenium Grid') {
      steps {
        step([$class: 'DockerComposeBuilder', dockerComposeFile: 'docker-compose_native.yml', option: [$class: 'StartAllServices'], useCustomDockerComposeFile: true])
        sh 'docker ps'
      }
    }

    stage('Run Tests') {
      agent {
        docker {
          image 'maven:3.6.3-openjdk-14'
        }

      }
      steps {
        sh 'docker ps'
        sh 'mvn test -DexecutionAddress="localhost:4444" -DtargetOperatingSystem="Linux-64" -DmaximumPerformanceMode="true" -DtargetBrowserName="GoogleChrome" -Dtest="!%regex[.*checksum.*], !%regex[.*cucumber.*], !%regex[.*sikulix.*], !%regex[.*imageComparison.*], !%regex[.*FileActions.*], !%regex[.*TerminalActions.*], !%regex[.*localShell.*], !%regex[.*fullPageScreenshotWithHeader.*], !%regex[.*dbConnection.*], !%regex[.*Appium.*]"'
        
      }
    }

    stage('Teardown') {
      steps {
        sh 'docker ps'
        step([$class: 'DockerComposeBuilder', dockerComposeFile: 'docker-compose_native.yml', option: [$class: 'StartAllServices'], useCustomDockerComposeFile: true])
      }
    }

  }
}
