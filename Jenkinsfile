pipeline {
    environment {
              APP_NAME = "register-app-pipeline"
    }
    agent any
    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
               steps {
                   git branch: 'amen', credentialsId: 'GitHub', url: 'https://github.com/amen123456/gitops-register-app-main'
               }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                   cat deployment.yaml
                   sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                   cat deployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "amen123456"
                   git config --global user.email "amenallah.benkhalifa@esprit.tn"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'GitHub', gitToolName: 'Default')]) {
                  sh "git push https://github.com/amen123456/gitops-register-app-main"
                }
            }
        }
      
    }
}
