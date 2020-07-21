pipeline {
  agent any
  stages {
    stage('Setup Selenium Grid') {
      steps {
        sh 'docker-compose -f docker-compose_native.yml up --scale chrome=4 --remove-orphans -d'
        step([$class: 'DockerComposeBuilder', dockerComposeFile: 'docker-compose_native.yml', option: [$class: 'StartAllServices'], useCustomDockerComposeFile: true])
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
        step([$class: 'DockerComposeBuilder', dockerComposeFile: 'docker-compose_native.yml', option: [$class: 'StopAllServices'], useCustomDockerComposeFile: true])
      }
    }

    stage('Setup docker-compose') {
      steps {
        sh 'curl -L https://github.com/docker/compose/releases/download/1.21.2/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose && chmod +x /usr/local/bin/docker-compose'
      }
    }

  }
}