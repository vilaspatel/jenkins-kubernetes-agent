pipeline {
  agent {
    kubernetes {
      label 'promo-app'  // all your pods will be named with this prefix, followed by a unique id
      idleMinutes 5  // how long the pod will live after no jobs have run on it
      yamlFile 'build-pod.yaml'  // path to the pod definition relative to the root of our project 
      defaultContainer 'maven'  // define a default container if more than a few stages use it, will default to jnlp container
    }
  }
  stages {
    stage('Build') {
      steps {  // no container directive is needed as the maven container is the default
        sh "mvn clean package"   
      }
    }
    stage('sonar-scanner') {
      steps { 
        container('sonar-scanner') {  
        sh "ls -lh"    
        sh "sonar-scanner -Dsonar.login=6ff25c2c9d25fb30c5fe0d41bae2528bfda5d115 -Dsonar.host.url=http://52.170.253.153:9000/ -Dsonar.projectKey=vilasproject -Dsonar.sources=. -X"   
      }
    }
    }  
//    stage('Build Docker Image') {
//      steps {
//        container('docker') {  
//          sh "docker build -t vividlukeloresch/promo-app:dev ."  // when we run docker in this step, we're running it via a shell on the docker build-pod container, 
//         sh "docker push vividlukeloresch/promo-app:dev"        // which is just connecting to the host docker deaemon
//       }
//     }
//   }
  }
}
