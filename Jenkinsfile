pipeline {
  agent none //start with no agent especified at the pipeline level 
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  stages {
    stage('Clone Repository') {
      agent {
        label "local"
      }
      steps {
        sh './gradlew clean check --no-daemon'
      }
    }
    stage('Hello agent 2') {
      // when {
      //    branch "dev-*"
      // }
      agent {
        label "agent2"
      }
      // steps {
      //    echo "Hello Alejandro Cardoso you are in branch dev-456"
      // }
      steps {
       echo "Hello agent2" 
      }
    }
    stage('Hello agent 1'){
      agent {
        label "agent1"
      }
      steps {
        echo "Hello agent1"
      }
    }
    stage ('Hello agent local'){
      agent {
        label "local"
      }
      steps {
        echo "Hello agent local"
      }
    }
      

  }
  post {
    always {
        node ("local"){
          junit(
          allowEmptyResults: true, 
          testResults: '**/build/test-results/test/*.xml'
          )
        }
    }
  }
}
