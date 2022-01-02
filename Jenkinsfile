node {
    def registryProject='localhost:5000'
    def IMAGE="${registryProject}/wartest-${env.BUILD_ID}"
    
    stage('Clone') {
        git 'https://github.com/priximmo/war-build-docker.git'
    }
    stage('Maven Package') {
        sh 'mvn package'
    }

    def img = stage('Build') {
        docker.build("$IMAGE", '.')
    }

    stage('Run') {
        img.withRun('--name run-$BUILD_ID -p 8081:8080') { c ->
            sh 'docker ps'
            sh 'netstat -ntaup'
            sh 'sleep 30s'
            sh 'curl 127.0.0.1:8081'
            sh 'docker ps'
        }
    }
    stage('Push') {
        docker.withRegistry('https://localhost:5000', '') {
            img.push 'latest'
            img.push()
        }
    }
}
