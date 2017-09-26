podTemplate(label: 'jenkins-pipeline', containers: [
    containerTemplate(name: 'jnlp', image: 'jenkinsci/jnlp-slave:2.62', args: '${computer.jnlpmac} ${computer.name}', workingDir: '/home/jenkins', resourceRequestCpu: '200m', resourceLimitCpu: '200m', resourceRequestMemory: '256Mi', resourceLimitMemory: '256Mi'),
    containerTemplate(name: 'docker', image: 'docker:1.12.6',       command: 'cat', ttyEnabled: true),
    containerTemplate(name: 'kubectl', image: 'lachlanevenson/k8s-kubectl:v1.4.8', command: 'cat', ttyEnabled: true)
],
volumes:[
    hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock'),
])
{
node ('jenkins-pipeline') {

    def app

    stage('Clone') {
       
        checkout scm
    }
    stage ('Build') {
        container('docker') {
        sh 'docker login -u fancy -p r=SIM=V/OY+/g+/v=h/X=C=+8f6itDz9 fancy.azurecr.io'
        sh 'cat Dockerfile'
        sh 'docker build -t node-web-app .'
        sh 'docker tag node-web-app fancy.azurecr.io/node-web-app:${env.BUILD_NUMBER}'
        sh 'docker push fancy.azurecr.io/node-web-app:${env.BUILD_NUMBER}'
            
        }
    }
    }
}
