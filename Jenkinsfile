pipeline {
    agent { label "Jenkins-Agent" }
    environment {
              APP_NAME = "register-app-pipeline"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
               steps {
                   git branch: 'main', credentialsId: 'github', url: 'https://github.com/OmarAdawy17/Gitops'
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
        script {
            sh """
               git config --global user.name "OmarAdawy17"
               git config --global user.email "OmarAdawy17o@gmail.com"
               git add deployment.yaml
               git diff --cached --quiet || git commit -m "Updated Deployment Manifest"
            """
            withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                sh "git push https://github.com/OmarAdawy17/Gitops main"
            }
        }
    }
}

      
    }
}
