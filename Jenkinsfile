pipeline {
  agent { label 'jenkins-node' }

  triggers { pollSCM('* * * * *') }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', 
        url: 'https://github.com/nolmbiiok/source-maven-java-spring-hello-webapp.git'
      }
    }
    stage('Build') {
      steps {
        sh 'mvn clean package'
      }
    }
   stage('Deploy') {
      steps {
        deploy adapters: [tomcat9(credentialsId: 'tomcat-manager', url: 'http://192.168.56.102:8080')], contextPath: null, war: 'target/hello-world.war'
      }
    }


    stage('Inspect WAR') {
      steps {
        sh '''
          mkdir -p tmpwar
          cd tmpwar
          jar xf ../target/hello-world.war
          echo "===================="
          echo "WAR 안의 index.jsp"
          echo "===================="
          cat WEB-INF/views/index.jsp
          echo "===================="
        '''
      }
    }
  }
}
