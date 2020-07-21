pipeline {
  agent any
  stages {
    stage('Setup docker-compose') {
      steps {
        sh '''curl -L https://github.com/docker/compose/releases/download/1.11.2/docker-compose-`uname -s`-`uname -m` > ~/docker-compose
chmod +x ~/docker-compose
mv ~/docker-compose /usr/local/bin/docker-compose
docker-compose run test'''
      }
    }

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