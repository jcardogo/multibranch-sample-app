// this pipeline is intended to beploy different services depending on the modification on each branch to affect different agents using each agens as a different environment
pipeline {
  agent none //start with no agent especified at the pipeline level 
  
  environment {
    GITHUB_CREDENTIALS= env.Github_PAT //use jenkins controller configured credential 
    GITHUB_REPO= 'jcardogo/AlejandroCardoso_website' //repository identification
  }
  
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }

  stages {
    stage('Hello agent 2') {
      when {
         branch "dev-*"
      }
      agent {
        label "agent2"
      }
      steps {
         echo "Hello Alejandro Cardoso you are in branch dev-456 deploying on agent2"
      }
    }
    
    stage('Clone Repository') { //bringing the html repo to the agent node
      when {
         branch "dev-*"
      }
      agent {
        label "agent2"
      }
      steps {
        checkout([
            $class: 'GitSCM', 
            branches: [[name: '*/main']], 
            extensions: [[$class: 'CleanBeforeCheckout']], 
            userRemoteConfigs: [[
              credentialsId: env.GITHUB_CREDENTIALS, 
              url: "https://github.com/${env.GITHUB_REPO}.git"
              ]]
          ])
        }
      }
    
    stage('Building Docker Image') { //creating a docker image with latest nginx service
      when {
         branch "dev-*"
      }
      agent {
        label "agent2"
      }
      steps {
        script{
         def nginxVersion = sh(returnStdout: true, script: 'curl -s https://hub.docker.com/v2/repositories/library/nginx/tags | jq -r \'$.results[].name\' | grep -v -E \'(alpine|window|perl)\' | sort -V | tail -n 1').trim() // Get latest stable tag
                    echo "Using Nginx version: ${nginxVersion}"
                    sh "docker build -t myWeb-nginx-image:latest --build-arg NGINX_VERSION=${nginxVersion} . -f Dockerfile" // Build with build arg
          
        }
      }
    }

    stage('Deploy Nginx Container') {
      when {
         branch "dev-*"
      }
      agent {
        label "agent2"
      }
      steps {
        sh 'docker run -d -p 8081:80 -v $(pwd)/html:usr/'
      }
    }

    stage('Verify Deployment'){
      when {
         branch "dev-*"
      }
      agent {
        label "agent2"
      }
      steps {
        sh 'curl http://localhost:8081'
      }

    }

  }
  post {
    always {
        node ("agent2"){
          junit(
          allowEmptyResults: true, 
          testResults: '**/build/test-results/test/*.xml'
          )
        }
    }
  }
}
