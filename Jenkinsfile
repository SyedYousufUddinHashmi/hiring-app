pipeline {
    agent any

        environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {
        
       
        stage('Docker Build') {
            steps {
                sh "docker build . -t hashmi6514/hiring-app:$BUILD_NUMBER"
            }
        }
        stage('Docker Push') {
            steps {
                withCredentials([string(credentialsId: 'hashmi6514', variable: 'hubPwd')]) {
                    sh "docker login -u hashmi6514 -p ${hubPwd}"
                    sh "docker push hashmi6514/hiring-app:$BUILD_NUMBER"
                }
            }
        }
        stage('Checkout K8S manifest SCM'){
            steps {
              git branch: 'main', url: 'https://github.com/SyedYousufUddinHashmi/Hiring-app-argocd1.git'
            }
        } 
        stage('Update K8S manifest & push to Repo'){
            steps {
                script{
                    withCredentials([string(credentialsId: 'githubtoken', variable: 'GIT_TOKEN')]) { 
                        sh '''
                        git config --global user.email Yousufhashmi20@gmail.com
                        git config --global user.name SyedYousufUddinHashmi
                        cat /var/lib/jenkins/workspace/$JOB_NAME/dev/deployment.yaml
                        sed -i "s/2/${BUILD_NUMBER}/g" /var/lib/jenkins/workspace/$JOB_NAME/dev/deployment.yaml
                        cat /var/lib/jenkins/workspace/$JOB_NAME/dev/deployment.yaml
                        git add .
                        git commit -m 'Updated the deploy yaml | Jenkins Pipeline'
                        git remote -v
                        git push https://$GIT_TOKEN@github.com/SyedYousufUddinHashmi/Hiring-app-argocd1.git main
                        '''                        
                      }
                  }
            }   
        }
            }
} 
