pipeline {
  agent none //start with no agent especified at the pipeline level 
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  stages {
    stage('Clone Repository') {
      steps {
        sh './gradlew clean check --no-daemon'
      }
      agent {
        when {
          branch "dev-*"
        }
        label "Built-In Node: 'default-agent'"
      }
    }
    stage('Hello') {
      when {
         branch "dev-*"
      }
      agent {
        when {
          branch "dev-*"
        }
        label "Ububtu_jenkins: 'default-agent'"
      }
      steps {
         echo "Hello Alejandro Cardoso you are in branch dev-456"
      }
    }
  }
  post {
    always {
        junit(
          allowEmptyResults: true, 
          testResults: '**/build/test-results/test/*.xml'
        )
    }
  }
}
