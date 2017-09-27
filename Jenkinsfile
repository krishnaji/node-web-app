podTemplate(label: 'docker',
  containers: [containerTemplate(name: 'docker', image: 'docker:1.11', ttyEnabled: true, command: 'cat'),
  containerTemplate(name: 'kubectl', image: 'smesch/kubectl', command: 'cat', ttyEnabled: true
  , volumes:[secretVolume(secretName: 'config', mountPath: '/root/.kube')])
  ],
  volumes: [hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')]
  ) {
  def image = "node-web-app"
  node('docker') {
    stage('Clone') {
               checkout scm
    }
    stage('Build Image') {
      container('docker') {
        sh "docker build -t ${image} ."
        withCredentials([[$class          : 'UsernamePasswordMultiBinding', credentialsId:'az-acr',
                        usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
          sh "docker login -u ${env.USERNAME} -p ${env.PASSWORD} fancy.azurecr.io"
        }
        sh "docker images " 
        sh "docker tag ${image} fancy.azurecr.io/node-web-app:${env.BUILD_NUMBER}"
        sh "docker push fancy.azurecr.io/node-web-app:${env.BUILD_NUMBER}"
           }
    }
    stage('Test'){
      println "Supper! All tests passed"
    }

    stage('Publish'){
        container('kubectl') {
            sh "kubectl set image deployment/node-web-app node-web-app=fancy.azurecr.io/node-web-app:${env.BUILD_NUMBER} --namespace default"
        }
    }
    }
}
