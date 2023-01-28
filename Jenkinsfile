pipeline {
  agent any
  stages {
    stage('compile') {
      steps {
        sh './mvnw clean compile'
        echo 'Jenkin Job started'
      }
    }

    stage('Static Analysis') {
      steps {
        sh '''./mvnw clean verify -DskipTests=true sonar:sonar \\
  -Dsonar.projectKey=spring-pet-clinic \\
  -Dsonar.host.url=http://3.110.109.106:9000 \\
  -Dsonar.login=sqp_f14e0b5f8560f745b8fa68a5b7eff5cd8be5f35e'''
      }
    }

    stage('Unit test') {
      steps {
        sh './mvnw "-Dtest=**/petclinic/*/*.java" test'
      }
    }

    stage('Package') {
      steps {
        sh './mvnw package -DskipTests=true'
      }
    }

    stage('Deploy') {
      agent {
        node {
          label 'charu-execution-node2'
        }

      }
      steps {
        sh './mvnw spring-boot:run </dev/null &>/dev/null &'
      }
    }

  }
}