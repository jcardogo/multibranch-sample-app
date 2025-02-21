pipeline {
  agent none //start with no agent especified at the pipeline level 
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  stages {
    stage('Clone Repository') {
      agent {
        label "Built-In Node: 'default-agent'"
      }
      steps {
        sh './gradlew clean check --no-daemon'
      }
    }
    stage('Hello') {
      when {
         branch "dev-*"
      }
      agent {
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
