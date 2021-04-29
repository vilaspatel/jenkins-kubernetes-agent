pipeline {
  agent {
    kubernetes {
      label 'promo-app'  // all your pods will be named with this prefix, followed by a unique id
      idleMinutes 5  // how long the pod will live after no jobs have run on it
      yamlFile 'build-pod.yaml'  // path to the pod definition relative to the root of our project 
      defaultContainer 'maven'  // define a default container if more than a few stages use it, will default to jnlp container
    }
  }
  environment {
    sonar_login     = credentials('sonar_login')
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
        //sh "ls -lh"
        sh "env"
        sh "sonar-scanner -Dsonar.login=$sonar_login -Dsonar.host.url=http://52.190.40.168 -Dsonar.projectKey=sonarqube-test -Dsonar.sources=. -Dsonar.java.binaries=target/my-app-1.0-SNAPSHOT.jar -X"   
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
