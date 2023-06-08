pipeline {
  environment{
  registry = "ajenemy/flask_app"
  registryCredentials = "docker"
  cluster_name = "skillstorm"
  namespace = "ajenemy"
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

    stage('Doicker login') {
      steps {
        sh 'docker login -u ajenemy -p dckr_pat_A3atVgkk_JoZ_5MVNf8NlOmb3qw'
      }
    }
  }
// This allows us to deploy kubernetes as a stage in Jenkins
  stage('kubernetes') {
    steps {
      withCredentials([aws(accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'AWS', secretKeyVariable: "AWS_SECRET_ACCESS_KEY")]) {
        sh "aws eks --region us-east-1 update-kubeconfig --name ${cluster_name}"
        script {
          try {
            sh "kubectl create namespace ${namespace}"
          }
          catch (Exception e) {
            echo "Error / namespace already created"
          }
        }
        sh "kubectl apply -f ./deployment.yaml -n ${namespace}"
        sh "kubectl -n ${namespace} rollout restart deployment ajenemyflask"
      }
    }
  }
  }
}