podTemplate(label: 'docker',
  containers: [containerTemplate(name: 'docker', image: 'docker:1.11', ttyEnabled: true, command: 'cat')],
  volumes: [hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')]
  ) {
  def image = "node-web-app"
  node('docker') {
    stage('Clone') {
               checkout scm
    }
    stage('Build Docker image') {
      container('docker') {
        sh "docker build -t ${image} ."
        withCredentials([[$class          : 'UsernamePasswordMultiBinding', credentialsId:'az-acr',
                        usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
          sh "docker login -u ${env.USERNAME} -p ${env.PASSWORD} fancy.azurecr.io"
        }
        sh "docker images " 
        sh "docker tag ${image} fancy.azurecr.io/node-web-app:${env.GIT_COMMIT }"
        sh "docker push fancy.azurecr.io/node-web-app:${env.GIT_COMMIT}"
           }
    }
    }
}
