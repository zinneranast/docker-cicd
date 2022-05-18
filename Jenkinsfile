node { // On which node this pipeline will run
   def commit_id // Create variables. Commit_id is exposed automatically by jenkins
   stage('Preparation') { // Job STAGE’E
     checkout scm // Check out this repo
     sh "git rev-parse --short HEAD > .git/commit-id"  // Get the latest commit-id                       
     commit_id = readFile('.git/commit-id').trim() // store the commit-id in the defined variable  
   }
   stage('test') { // Stage TEST
     nodejs(nodeJSInstallationName: 'nodejs') {
       sh 'npm install --only=dev' // RUN npm install dev  
       sh 'npm test' // RUN npm tests
     }
   }
   stage('docker build/push') { // Stage build & push
     docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') { // used docker hub api and ‘docker hub’ 
     									          credintials   
       def app = docker.build("repo/imagename:${commit_id}", '.').push()
     }
   }
}
