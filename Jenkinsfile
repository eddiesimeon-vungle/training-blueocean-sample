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
        archiveArtifacts 'target/*.war'
      }
    }
    stage('test') {
      steps {
        parallel(
          "backend": {
            sh './jenkins/test-backend.sh'
            junit '**/surefire-reports/**/*.xml'
            
          },
          "frontend": {
            sh './jenkins/test-frontend.sh'
            junit '**/test-results/karma/*.xml'
            
          },
          "static": {
            sh './jenkins/test-static.sh'
            
          },
          "performance": {
            sh './jenkins/test-performance.sh'
            
          }
        )
      }
    }
    stage('deploy to staging') {
      steps {
        sh './jenkins/deploy.sh staging'
      }
    }
    stage('deploy to production') {
      steps {
        input(message: 'Deploy to production?', ok: 'fire away')
        sh './jenkins/deploy.sh staging'
        sh 'echo Notifying appropriate team members!'
      }
    }
  }
}