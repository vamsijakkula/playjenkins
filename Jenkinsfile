pipeline {

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git url:'https://github.com/vamsijakkula/playjenkins.git', branch:'master'
      }
    }
    
      stage("Build image") {
            steps {
                script {
                    myapp = docker.build("vamsijakkula/hellowhale:${env.BUILD_ID}")
                }
            }
        }
    
      stage('Test image') {
      steps {
        script {
          app.inside { 
            sh 'echo "Tests passed"'
          }
        }

      }
    }
        stage("Push image") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                            myapp.push("latest")
                            myapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }

    
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "nginx.yml", kubeconfigId: "mykubeconfig")
        }
      }
    }

  }

}
