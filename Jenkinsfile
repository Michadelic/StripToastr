pipeline {
  agent {
    docker {
      image 'node'
  }
  stages {
    stage('Checkout') {
      steps {
          // Used only for non-Multibranch Pipelines
          // git 'https://github.com/rtyler/jhipster-sample-app'
          checkout scm
          stash includes: '**', name: 'ws', useDefaultExcludes: false
      }
    }
    stage('Backend') {
        steps {
            parallel(
                'Unit' : {
                    unstash 'ws'
                    unstash 'war'
                    sh './mvnw -B test'
                    junit '**/surefire-reports/**/*.xml'
                },
                'Performance' : {
                    unstash 'ws'
                    unstash 'war'
                    // sh './mvnw -B gatling:execute'
                })
        }
    }
    stage('Test Frontend') {
        steps {
            unstash 'ws'
            sh 'yarn install'
            sh 'yarn global add gulp-cli'
            sh 'gulp test'
        }
    }

    stage('Deploy to Staging') {
        steps {
            echo "Let's pretend a deployment is happening"
        }
    }
    stage('Deploy to production') {
        steps {
            input message: 'Deploy to production?', ok: 'Fire zee missiles!'
            echo "Let's pretend a production deployment is happening"
        }
    }
  }