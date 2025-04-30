# WebApp - Project
## Steps :-
## **1. Create a instance :-**
![1](https://github.com/sudhajobs0107/WebApp-Project/blob/main/images/task1.1.png)
## **2. Connect instance to local(powershell) through ssh :-**
```
ssh -i "key" ubuntu@hostname
```
![2](https://github.com/sudhajobs0107/WebApp-Project/blob/main/images/task1.2.png)
## **3. Run command :-**
```
sudo apt update
```
## **4. Install docker  :-**
```
sudo apt update && sudo apt upgrade -y
sudo apt install docker.io –y
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker Ubuntu
docker –version
sudo reboot
```
![3](https://github.com/sudhajobs0107/WebApp-Project/blob/main/images/task1.3.png)
## **5. Install Jenkins :-**
```
sudo apt update && sudo apt upgrade -y
sudo apt install openjdk-17-jdk –y
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee   /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]   https://pkg.jenkins.io/debian-stable binary/ | sudo tee   /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt update
sudo apt install jenkins –y
sudo systemctl start Jenkins
sudo systemctl enable Jenkins
sudo usermod -aG docker Jenkins
systemctl restart Jenkins
sudo apt-get update
sudo systemctl status Jenkins
```
![4](https://github.com/sudhajobs0107/WebApp-Project/blob/main/images/task1.4.png)
## **6. Now copy **Public IPv4 address:8080** and we will be on **Unlock Jenkins page**. Make sure port 8080 is open if not please open port 8080.**
![5](https://github.com/sudhajobs0107/WebApp-Project/blob/main/images/task1.5.png)
## **7. To unlock jenkins, use command :-**
```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
## **8. And we will get our password. Copy and paste it to unlock Jenkins → Now click Install suggested plugins → Fill details → Welcome to Jenkins**
![6](https://github.com/sudhajobs0107/WebApp-Project/blob/main/images/task1.6.png)
## **9. Now to install trivy use command :-**
```
sudo apt-get install wget apt-transport-https gnupg lsb-release
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -
echo deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main | sudo tee -a /etc/apt/sources.list.d/trivy.list
sudo apt-get update
sudo apt-get install trivy
trivy –version
```
![7](https://github.com/sudhajobs0107/WebApp-Project/blob/main/images/task1.7.png)
## **10. Go to into repository → Settings → Webhooks → Add Webhook → Payload URL:- http://Public-IP:8080/github-webhook/ → Content type: application/json → Click Add Webhook**
## **11. Setup Email Integration With Jenkins. First we have to install email plugin. For this Go to Manage Jenkins → Plugins → Available Plugins → Search Email Extension Template → install this plugin. Now go to your Gmail → click on your profile → click on Manage Your Google Account → click on the Security tab on the left side panel → search App Passwords → Create a password.**
## **12. Now add Email Credentials in Jenkins → Go to Manage Jenkins → click on Credentials → System → Global credentials → Add Credentials → Username with password → in Secret put sudhajobs0107@gmail.com and password that we created earlier → ID "email" → Description "email" → Create.**
## **13. Now Go to Manage Jenkins → System → Find E-mail notification → Add SMTP=smtp.gmail.com → click Avanced → UserName=sudhajobs0107@gmail → Password=put that we created → Tick Use SSL → SMTP Port=465 → click on Apply.**
## **14. We have to add one more thing so in System → Find Extended E-mail Notification → Add SMTP=smtp.gmail.com → SMTP Port=465 → click Avanced → Cedentials=select email that we creted we earlier → Tick Use SSL → Default Content Type=HTML → go down and find Default Triggers → Tick Always →  click on Apply and Save.**
## **15. Now build a pipeline click on **Create a job** → give name → select "**Pipeline**" → click **OK**.**
## **16. Now add the script in Pipeline Script.**
```
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
```
## **17. Now click Apply and Save → **Build Now** and our pipeline will build successfully.**
![8](https://github.com/sudhajobs0107/WebApp-Project/blob/main/images/task1.8.png)
## **18. When pipeline "SUCCES" or "FAILURE" we will get email like image given below :-**
![9](https://github.com/sudhajobs0107/WebApp-Project/blob/main/images/task1.9.png)


# Our WebApp Project is completed :smile:.

