pipeline {
    agent any

    stages {
        
        stage("Code"){
            steps{
                git url: "https://github.com/sudhajobs0107/WebApp-Project.git" , branch: "main"
                echo "Code Cloned Successfully"
            }
        }
        
        stage("Build Docker Image"){
            steps{
                sh 'docker build -t my-webapp:latest .'
                echo "Code Built Successfully"
            }
        }
        
        stage("Trivy"){
            steps{
                sh "trivy image my-webapp"
            }
        }
        
        stage('Run Docker Container') {
            steps {
                sh 'docker run -d -p 5000:5000 my-webapp'
                echo "App Deployed Successfully"
            }
        }
    }

    post {
        always {
            emailext attachLog: true,
                subject: "'${currentBuild.result}'",
                body: "Project: ${env.JOB_NAME}<br/>" +
                    "Build Number: ${env.BUILD_NUMBER}<br/>" +
                    "URL: ${env.BUILD_URL}<br/>",
                to: 'sudhajobs0107@gmail.com', 
                attachmentsPattern: 'trivyfs.txt,trivyimage.txt'
        }
    }
}
