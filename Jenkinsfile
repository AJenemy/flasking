pipeline {
  agent any
  stages {
    stage('Git') {
      steps {
        git(url: 'https://github.com/AJenemy/flasking.git', branch: 'main', credentialsId: 'https://github.com/AJenemy/flasking.git')
      }
    }

  }
}