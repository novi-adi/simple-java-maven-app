node {
  stage('Build') {
    dir('simple-java-maven-app') {
      sh 'mvn clean package'
    }
  }

  stage('Test') {
    dir('simple-java-maven-app') {
      sh 'mvn test'
    }
  }
  
  stage('Cleanup') {
    deleteDir()
  }
}