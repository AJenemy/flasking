pipeline {
  environment{
  registry = "ajenemy/flasking"
  registryCredentials = "docker"
  cluster_name = "skillstorm"
}

  agent {
    node {
      label 'docker'
    }

  }
  stages {
    stage('Git') {
      steps {
        git(url: 'https://github.com/AJenemy/flasking.git', branch: 'main')
      }
    }

  stage ('Build stage') {
    steps {
      script {
        dockerImage = docker.build(registry)
      }
    }
  }

  stage('Deployment stage'){
    steps {
      script {
        docker.withRegistry('', registryCredentials) {
          dockerImmage.push()
        }
      }
    }
  }

    stage('Build') {
      steps {
        sh 'docker build -t ajenemy/flask_app .'
      }
    }

    stage('Docker login') {
      steps {
        sh 'docker login -u ajenemy -p dckr_pat_A3atVgkk_JoZ_5MVNf8NlOmb3qw'
      }
    }

    stage('Docker push') {
      steps {
        sh 'docker push ajenemy/flask_app'
      }
    }

  }
}