pipeline {
    agent { label "jenkins-agent" }
    environment {
              APP_NAME = "demo-dec-project-added"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
               steps {
                   git branch: 'main', credentialsId: 'github-credentials', url: 'https://github.com/aniwardhan/gitops-project.git'
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
                   git config --global user.name "Anitha"
                   git config --global user.email "aniwardhan@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github-credentials', gitToolName: 'Default')]) {
                  sh "git push https://github.com/aniwardhan/gitops-project.git main"
                }
            }
        }
      
    }
}
