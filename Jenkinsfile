pipeline {
  agent {
    docker {
      image 'bitwiseman/training-blueocean-sample'
      args '-u root -v $HOME/.m2:/root/.m2'
    }
    
  }
  stages {
    stage('build') {
      steps {
        sh './jenkins/build.sh'
      }
    }
  }
}