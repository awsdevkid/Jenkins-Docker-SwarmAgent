throttle(['throttleDocker']) {
  node('docker') {
    wrap([$class: 'AnsiColorBuildWrapper']) {
      try{
        stage('Setup') {
          checkout scm
          sh '''
            sudo sh ci/docker-down.sh
            sudo sh ci/docker-up.sh
          '''
        }
        stage('Test'){
          parallel (
            "unit": {
              sh '''
                sudo sh ci/test/unit.sh
              '''
            },
            "functional": {
              sh '''
                sudo sh ci/test/functional.sh
              '''
            }
          )
        }
        stage('Capacity Test') {
          sh '''
            sudo sh ci/test/stress.sh
          '''
        }
      }
      finally {
        stage('Cleanup') {
          sh '''
            sudo sh ci/docker-down.sh
          '''
        }
      }
    }
  }
}
