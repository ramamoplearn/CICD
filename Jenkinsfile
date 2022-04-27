pipeline {
  agent any
  stages {
    stage('Source') {
      steps {
        echo 'Start of pipeline'
        echo 'Second Step'
      }
    }

    stage('error') {
      steps {
        sleep 2
        echo 'Sleep for 2 seconds'
      }
    }

    stage('Build') {
      steps {
        echo 'In build in build'
      }
    }

  }
}
