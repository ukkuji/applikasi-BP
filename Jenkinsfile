properties([pipelineTriggers([githubPush()])]) 
pipeline {
    agent any
    stages {
        stage('Code Scanning') {
          agent { label "agent1" }
            steps {
              //
                script { echo "Test" 
                // def scannerHome = tool 'sonarscanner1' ;
	              // withSonarQubeEnv('sonarserver1') {
	              // sh "${scannerHome}/bin/sonar-scanner"
	              //   }
                }
              }
            }
        stage('Build') {
          agent { label "agent1" }
            steps {
              //
                script { echo "Build"

                if (env.BRANCH_NAME == "staging")
                
                { 
                // sh "pwd && ls -lah"
                sh "docker build -t andiukku/backend:stage-\$(git rev-parse --short HEAD) -t andiukku/backend:stage-latest -f backend/Dockerfile backend/."
                sh "docker build -t andiukku/frontend:stage-\$(git rev-parse --short HEAD) -t andiukku/frontend:stage-latest -f frontend/Dockerfile frontend/."

                sh "docker push andiukku/backend:stage-\$(git rev-parse --short HEAD)"
                sh "docker push andiukku/backend:stage-latest"
                sh "docker push andiukku/frontend:stage-\$(git rev-parse --short HEAD)"
                sh "docker push andiukku/frontend:stage-latest"

                }else{ 

                sh "pwd && ls -lah"
                sh "docker build -t andiukku/backend:prod-\$(git rev-parse --short HEAD) -t andiukku/backend:prod-latest -f backend/Dockerfile backend/."
                sh "docker build -t andiukku/frontend:prod-\$(git rev-parse --short HEAD) -t andiukku/frontend:prod-latest -f frontend/Dockerfile frontend/."

                sh "docker push andiukku/backend:prod-\$(git rev-parse --short HEAD)"
                sh "docker push andiukku/backend:prod-latest"
                sh "docker push andiukku/frontend:prod-\$(git rev-parse --short HEAD)"
                sh "docker push andiukku/frontend:prod-latest"
                
                }
                
                }
                
              }
            }
        stage('Deploy') {
          agent { label "agent1" }
            steps {
              //
                script { echo "Testing Deploy Lagi" 

                if (env.BRANCH_NAME == "staging")
                
                { 

                sh "kubectl -n staging set image deployment/backend-app backend-app=andiukku/backend:stage-\$(git rev-parse --short HEAD)"
                sh "kubectl -n staging set image deployment/frontend-app frontend-app=andiukku/frontend:stage-\$(git rev-parse --short HEAD)"
                sh "docker image rmi andiukku/backend:stage-\$(git rev-parse --short HEAD)"
                sh "docker image rmi andiukku/backend:stage-latest"
                sh "docker image rmi andiukku/frontend:stage-\$(git rev-parse --short HEAD)"
                sh "docker image rmi andiukku/frontend:stage-latest"
                }else{ 

                sh "kubectl -n production set image deployment/backend-app backend-app=andiukku/backend:prod-\$(git rev-parse --short HEAD)"
                sh "kubectl -n production set image deployment/frontend-app frontend-app=andiukku/frontend:prod-\$(git rev-parse --short HEAD)"
                sh "docker image rmi andiukku/backend:prod-\$(git rev-parse --short HEAD)"
                sh "docker image rmi andiukku/backend:prod-latest"
                sh "docker image rmi andiukku/frontend:prod-\$(git rev-parse --short HEAD)"
                sh "docker image rmi andiukku/frontend:prod-latest"
                }

                }
            }
          }
        }
      }