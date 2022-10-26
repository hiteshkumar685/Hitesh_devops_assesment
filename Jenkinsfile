/*6
This pipeline is used to sync git state to deployed state.
Any app.version file update will trigger this pipeline to sync state.
*/

///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
// CALCULATED VARIABLES
LABEL_NAME  = 'agent-' + new Date().getTime()
///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

pipeline {
    agent {
        kubernetes {
            label "$LABEL_NAME"
            yaml '''
apiVersion: v1
kind: Pod
spec:
  serviceAccountName: jenkins-sa
  containers:
  - name: app-builder
    image: docker:latest
    command: ['cat']
    tty: true
    volumeMounts:
    - name: dockersock
      mountPath: /var/run/docker.sock
  volumes:
  - name: dockersock
    hostPath:
      path: /var/run/docker.sock
'''
        }
    }
    environment {
      GIT_SSH_COMMAND = "ssh -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no"
      TAG = "${env.BUILD_NUMBER}"
      registryCredential  = 'Docker' # Add your dockerhub creds 
    }
    stages {
      stage('GitClone') {
        steps {
          container('app-builder') {
            script {
              git url: 'GIT URL' # Add your github link
            }
          }
        }
      }
      stage('Prepared') {
        steps {
          container('app-builder') {
            sh '''
              curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
              mv kubectl /usr/local/bin
              chmod +x /usr/local/bin/kubectl
            '''
          }
        }
      }
      stage('Build') {
        steps {
          container('app-builder') {
            script {
              docker.withRegistry( '', registryCredential ) {
                sh '''
                  docker build -t DOCKERHUB-REPO:sample-app-$TAG .
                  docker push DOCKERHUB-REPO:sample-app-$TAG
                '''
              }
            }
          }
        }
      }
      stage('Deploy') {
        steps {
          container('app-builder') {
            sh '''
			        kubectl apply -f sample-service/k8s-manifest/Deployment.yaml
              kubectl rollout restart deployment app-server --namespace=app
            '''
          }
        }
      }
    }
}
