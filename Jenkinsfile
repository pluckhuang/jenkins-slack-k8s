pipeline {
  agent any
  stages {
    stage('Docker Build') {
      steps {
        sh "docker build -t pluckhuang/podinfo:${env.BUILD_NUMBER} ."
      }
    }
    stage('Docker Push') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
          sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
          sh "docker push pluckhuang/podinfo:${env.BUILD_NUMBER}"
        }
      }
    }
    stage('Docker Remove Image') {
      steps {
        sh "docker rmi pluckhuang/podinfo:${env.BUILD_NUMBER}"
      }
    }
    stage('Apply Kubernetes Files') {
      steps {
          withKubeConfig([credentialsId: 'kubeconfig']) {
          sh 'cat deployment.yaml | sed "s/{{BUILD_NUMBER}}/$BUILD_NUMBER/g" | kubectl apply -f -'
          sh 'kubectl apply -f service.yaml'
        }
      }
  }
}
post {
    success {
      slackSend(channel: "#ok", message: "pluckhuang/podinfo:${env.BUILD_NUMBER} Pipeline is successfully completed.")
    }
    failure {
      slackSend(channel: "#error", message: "pluckhuang/podinfo:${env.BUILD_NUMBER} Pipeline failed. Please check the logs.")
    }
}
}
