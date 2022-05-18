node {
   def commit_id
   stage('Preparation') {
     checkout scm
     sh "git rev-parse --short HEAD > .git/commit-id"
     commit_id = readFile('.git/commit-id').trim()
   }
   stage('dockerbuild') {
     def app = docker.build("zinneranast/docker-nodejs-demo:${commit_id}", '.')
   }
   stage('sonarqube tests') {
      def app = docker.build("zinneranast/sonar:${commit_id}", '-f soanrbuild' , '.')
     }
     mysql.stop()
   }
   stage('docker build/push') {
     docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
       def app = docker.build("zinneranast/docker-nodejs-demo:${commit_id}", '.').push()
     }
   }
}
