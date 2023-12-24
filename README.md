# Deploy Netflix Clone on Cloud using Jenkins - DevSecOps Project!


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/6e520cbc-abe3-483e-84f5-9792c17ad36a)


#### Phase 1:
Initial Setup and Deployment

Step 1:
Launch EC2 (Ubuntu 22.04):

- Provision an EC2 instance on AWS with Ubuntu 22.04.

- Connect to the instance using SSH.

![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/882b3b31-b424-4384-86a9-2f09e94ce204)


Step 2:
Clone the Code:

- Update all the packages and then clone the code.

- Clone your application's code repository onto the EC2 instance:

git clone https://github.com/N4si/DevSecOps-Project.git


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/0b77e04d-cdfb-4c01-b97c-3b559d8d93c5)


Step 3:

- Install Docker and Run the App Using a Container

- Set up Docker on the EC2 instance:

- sudo apt-get update


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/77992070-0a58-4868-9bd9-5fffb2465931)


- sudo apt-get install docker.io -y


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/e15c8615-8aad-457c-8069-b396d863d78d)


- sudo usermod -aG docker $USER  # Replace with your system's username, e.g., 'ubuntu'

- newgrp docker

- sudo chmod 777 /var/run/docker.sock


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/fef4ae23-463f-4d09-83dc-8e62ac65470b)


Build and run your application using Docker containers:

- docker build -t netflix .


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/ce41a61f-ed52-4fe2-b4eb-1c72c24d034e)


- open necessary ports for your app


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/19d3d466-05ba-4edb-9113-d13ed12732df)


- docker run -d --name netflix -p 8081:80 netflix:latest


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/0e149ade-aec9-4d91-a519-00acd82fce78)


- with your local ip and port opening test if it works, you should notice it has no content


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/0055e5f1-5127-4f23-a370-4366c089d216)


to delete
docker stop <containerid>
docker rmi -f netflix
It will show an error cause you need API key

Step 4:
#### Get the API Key:

- Open a web browser and navigate to TMDB (The Movie Database) website.

- Click on "Login" and create an account.

- Once logged in, go to your profile and select "Settings."

- Click on "API" from the left-side panel.

- Create a new API key by clicking "Create" and accepting the terms and conditions.

- Provide the required basic details and click "Submit."

- You will receive your TMDB API key.

#### Now recreate the Docker image with your api key:

- docker build --build-arg TMDB_V3_API_KEY=<your-api-key> -t netflix .


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/b00ad155-aec0-4000-aa0c-6f6154250c02)


- run the latest image


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/a530ebf0-de86-4752-a69b-7f46c7caade9)


- reload the localhost


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/b8793850-7641-49da-8f24-83fc8595555d)


#### Phase 2:
Security

- Install SonarQube and Trivy:




- Install SonarQube and Trivy on the EC2 instance to scan for vulnerabilities.


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/f848a383-d42e-4b93-806e-940f9b1791c8)


sonarqube

`docker run -d --name sonar -p 9000:9000 sonarqube:lts-community`

To access:

publicIP:9000 (by default username & password is admin)


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/f99a5719-a31e-459d-b52a-1d978e64e1f1)



![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/0ac12ee3-084d-41de-b4ca-e65f850b1261)


To install Trivy:

`sudo apt-get install wget apt-transport-https gnupg lsb-release
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -
echo deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main | sudo tee -a /etc/apt/sources.list.d/trivy.list
sudo apt-get update
sudo apt-get install trivy`        


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/4eddd45a-e6b2-4888-ae10-2b04ca82fa6b)


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/dd17e2e8-99b5-4d22-86a8-e6863ed72ab3)



- try to scan image using trivy

![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/030e3b6a-7300-4e7a-951f-d06e08a82d73)


- trivy image <imageid>

#### Integrate SonarQube and Configure:

- Integrate SonarQube with your CI/CD pipeline.

- Configure SonarQube to analyze code for quality and security issues.
Phase 3: CI/CD Setup

- Install Jenkins for Automation:

- Install Jenkins on the EC2 instance to automate deployment: Install Java

`sudo apt update
sudo apt install fontconfig openjdk-17-jre
java -version
openjdk version "17.0.8" 2023-07-18
OpenJDK Runtime Environment (build 17.0.8+7-Debian-1deb12u1)
OpenJDK 64-Bit Server VM (build 17.0.8+7-Debian-1deb12u1, mixed mode, sharing)`

####jenkins

`sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
/etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
sudo systemctl start jenkins
sudo systemctl enable jenkins`


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/376ee674-dfdb-465b-b013-152494c1d5eb)


- Access Jenkins in a web browser using the public IP of your EC2 instance.

- publicIp:8080


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/4c52552c-343e-4940-9b85-3c52eac05a70)


- Install Necessary Plugins in Jenkins:

- Goto Manage Jenkins →Plugins → Available Plugins →

#### Install below plugins

-  Eclipse Temurin Installer (Install without restart)

-  SonarQube Scanner (Install without restart)

-  NodeJs Plugin (Install Without restart)

-  Email Extension Plugin

#### Configure Java and Nodejs in Global Tool Configuration

- Goto Manage Jenkins → Tools → Install JDK(17) and NodeJs(16)→ Click on Apply and Save

![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/4a613d67-91d8-4bdb-a55c-fdd4247fec7d)


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/1076e77c-05fd-41d2-9140-441553d5471f)


#### SonarQube
- Create the token

- Goto Jenkins Dashboard → Manage Jenkins → Credentials → Add Secret Text. It should look like this
After adding sonar token

![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/9d65341e-313a-4e22-afc1-b7ca2dfda2b1)

Click on Apply and Save

The Configure System option is used in Jenkins to configure different server
Global Tool Configuration is used to configure different tools that we install using Plugins
We will install a sonar scanner in the tools.


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/c33543c9-1541-4263-8321-1a1a33f704fd)


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/9c51b171-e77b-479b-93d6-06c4e5dd2cb4)


#### Create a Jenkins webhook

Configure CI/CD Pipeline in Jenkins:

- Create a CI/CD pipeline in Jenkins to automate your application deployment.


`pipeline {
    
    agent any
    tools {
        
        jdk 'jdk17'
        nodejs 'node16'
    }
    
    environment {
        SCANNER_HOME = tool 'sonar-scanner'
    }
    stages {
        
        stage('clean workspace') {
            steps {
                cleanWs()
            }
        }
        
        stage('Checkout from Git') {
            steps {
                git branch: 'main', url: 'https://github.com/N4si/DevSecOps-Project.git'
            }
        }
        
        stage("Sonarqube Analysis") {
            steps {
                withSonarQubeEnv('sonar-server') {
                    sh '''$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Netflix \
                    -Dsonar.projectKey=Netflix'''
                }
            }
        }
        
        stage("quality gate") {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'Sonar-token'
                }
            }
        }
        
        stage('Install Dependencies') {
            steps {
                sh "npm install"
            }
        }
    }
}`

![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/193bacad-b131-45f3-8459-6e524d97662f)


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/79ae7d03-9b20-4f1e-a7b8-6360217fdc4d)


Certainly, here are the instructions without step numbers:

- Install Dependency-Check and Docker Tools in Jenkins

- Install Dependency-Check Plugin:

- Go to "Dashboard" in your Jenkins web interface.

- Navigate to "Manage Jenkins" → "Manage Plugins."

- Click on the "Available" tab and search for "OWASP Dependency-Check."

- Check the checkbox for "OWASP Dependency-Check" and click on the "Install without restart" button.
 Configure Dependency-Check Tool:

#### After installing the Dependency-Check plugin, you need to configure the tool.

- Go to "Dashboard" → "Manage Jenkins" → "Global Tool Configuration."

- Find the section for "OWASP Dependency-Check."

- Add the tool's name, e.g., "DP-Check."

- Save your settings.


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/bc3bffe5-f2f5-4721-bb73-6774ff547b55)


#### Install Docker Tools and Docker Plugins:

- Go to "Dashboard" in your Jenkins web interface.

- Navigate to "Manage Jenkins" → "Manage Plugins."

- Click on the "Available" tab and search for "Docker."

- Check the following Docker-related plugins:

- Docker

- Docker Commons

- Docker Pipeline

- Docker API

- docker-build-step

Click on the "Install without restart" button to install these plugins.

#### Add DockerHub Credentials:

To securely handle DockerHub credentials in your Jenkins pipeline, follow these steps:

- Go to "Dashboard" → "Manage Jenkins" → "Manage Credentials."

Click on "System" and then "Global credentials (unrestricted)."

Click on "Add Credentials" on the left side.

Choose "Secret text" as the kind of credentials.

Enter your DockerHub credentials (Username and Password) and give the credentials an ID (e.g., "docker").

Click "OK" to save your DockerHub credentials.


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/5fe557de-ec53-4d60-9293-4cd0a1ffafea)


Now, you have installed the Dependency-Check plugin, configured the tool, and added Docker-related plugins along with your DockerHub credentials in Jenkins. You can now proceed with configuring your Jenkins pipeline to include these tools and credentials in your CI/CD process.

![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/f36f486b-d5a1-4c09-88cb-a15cb8299fe8)


- stop and remove the netflix running image

![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/8cdd5ca3-05cb-4fb0-a4ff-fd5d995e64e3)


`
pipeline{
    agent any
    
    tools{
        
        jdk 'jdk17'
        nodejs 'node16'
    }
    
    environment {
        
        SCANNER_HOME=tool 'sonar-scanner'
    }
    
    stages {
        stage('clean workspace'){
            steps{
                cleanWs()
            }
        }
        
        stage('Checkout from Git'){
            steps{
                git branch: 'main', url: 'https://github.com/N4si/DevSecOps-Project.git'
            }
        }
        
        stage("Sonarqube Analysis "){
            steps{
                withSonarQubeEnv('sonar-server') {
                    sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Netflix \
                    -Dsonar.projectKey=Netflix '''
                }
            }
        }
        
        stage("quality gate"){
           steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'Sonar-token' 
                }
            } 
        }
        
        stage('Install Dependencies') {
            steps {
                sh "npm install"
            }
        }
        
        stage('OWASP FS SCAN') {
            steps {
                dependencyCheck additionalArguments: '--scan ./ --disableYarnAudit --disableNodeAudit', odcInstallation: 'DP-Check'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        
        stage('TRIVY FS SCAN') {
            steps {
                sh "trivy fs . > trivyfs.txt"
            }
        }
        
        
        stage("Docker Build & Push"){
            steps{
                script{
                   withDockerRegistry(credentialsId: 'docker', toolName: 'docker'){   
                       sh "docker build --build-arg TMDB_V3_API_KEY=<yourapikey> -t netflix ."
                       sh "docker tag netflix uzondu/netflix:latest "
                       sh "docker push uzondu/netflix:latest "
                    }
                }
            }
        }
        
        stage("TRIVY"){
            steps{
                sh "trivy image uzondu/netflix:latest > trivyimage.txt" 
            }
        }
        
        stage('Deploy to container'){
            steps{
                sh 'docker run -d --name netflix -p 8081:80 nasi101/netflix:latest'
            }
        }
    }
}`





If you get docker login failed errorr

` sudo su
 sudo usermod -aG docker jenkins
 sudo systemctl restart jenkins `


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/63d4b730-21ae-40ba-92ae-950c4fad2de9)


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/59b4fb64-6769-43e4-886f-113773a0a97f)


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/3470ea88-5734-4fd6-b6e5-091bf649429b)



Phase 4:
#### Monitoring

- Install Prometheus and Grafana:

- Set up Prometheus and Grafana to monitor your application.

- Installing Prometheus:

- First, create a dedicated Linux user for Prometheus and download Prometheus:


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/f18befea-5e39-447c-9db9-ba2a8f0d6089)


- sudo useradd --system --no-create-home --shell /bin/false prometheus


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/fd0175a6-1b4c-402d-944d-a2cf83745459)


- wget https://github.com/prometheus/prometheus/releases/download/v2.47.1/prometheus-2.47.1.linux-amd64.tar.gz

- Extract Prometheus files, move them, and create directories:


` tar -xvf prometheus-2.47.1.linux-amd64.tar.gz
 cd prometheus-2.47.1.linux-amd64/
 sudo mkdir -p /data /etc/prometheus
 sudo mv prometheus promtool /usr/local/bin/
 sudo mv consoles/ console_libraries/ /etc/prometheus/
 sudo mv prometheus.yml /etc/prometheus/prometheus.yml `


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/3e167423-ffa3-4291-a3f1-b3acd7d2eacd)

  
#### Set ownership for directories:

sudo chown -R prometheus:prometheus /etc/prometheus/ /data/
Create a systemd unit configuration file for Prometheus:

sudo nano /etc/systemd/system/prometheus.service
Add the following content to the prometheus.service file:



[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

StartLimitIntervalSec=500
StartLimitBurst=5

[Service]
User=prometheus
Group=prometheus
Type=simple
Restart=on-failure
RestartSec=5s
ExecStart=/usr/local/bin/prometheus \
  --config.file=/etc/prometheus/prometheus.yml \
  --storage.tsdb.path=/data \
  --web.console.templates=/etc/prometheus/consoles \
  --web.console.libraries=/etc/prometheus/console_libraries \
  --web.listen-address=0.0.0.0:9090 \
  --web.enable-lifecycle
  
[Install]
WantedBy=multi-user.target

![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/e9edb8d3-936b-4c48-b440-263b7436c0fb)


Here's a brief explanation of the key parts in this prometheus.service file:

User and Group specify the Linux user and group under which Prometheus will run.

ExecStart is where you specify the Prometheus binary path, the location of the configuration file (prometheus.yml), the storage directory, and other settings.

web.listen-address configures Prometheus to listen on all network interfaces on port 9090.

web.enable-lifecycle allows for management of Prometheus through API calls.

- Enable and start Prometheus:

- sudo systemctl enable prometheus

- sudo systemctl start prometheus

- Verify Prometheus's status:

- sudo systemctl status prometheus

![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/32363d43-c336-4b7c-b53a-2f0e6c6f6bf2)


- You can access Prometheus in a web browser using your server's IP and port 9090:

http://<your-server-ip>:9090


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/36aaf9ae-e01d-4b35-a47e-413cfd8b8b22)



#### Installing Node Exporter:

Create a system user for Node Exporter and download Node Exporter:

` sudo useradd --system --no-create-home --shell /bin/false node_exporter
 wget https://github.com/prometheus/node_exporter/releases/download/v1.6.1/node_exporter-1.6.1.linux-amd64.tar.gz
 Extract Node Exporter files, move the binary, and clean up: `


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/7a3bb856-9d15-44be-9d2a-ada54ca32c89)


` tar -xvf node_exporter-1.6.1.linux-amd64.tar.gz
 sudo mv node_exporter-1.6.1.linux-amd64/node_exporter /usr/local/bin/
 rm -rf node_exporter* `


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/206f6b9f-0847-4782-b48b-89c9cee3b70b)


Create a systemd unit configuration file for Node Exporter:

- sudo nano /etc/systemd/system/node_exporter.service

- Add the following content to the node_exporter.service file:

[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target

StartLimitIntervalSec=500
StartLimitBurst=5

[Service]
User=node_exporter
Group=node_exporter
Type=simple
Restart=on-failure
RestartSec=5s
ExecStart=/usr/local/bin/node_exporter --collector.logind

[Install]
WantedBy=multi-user.target
Replace --collector.logind with any additional flags as needed.


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/245e3fa9-8625-43dd-a6df-1873253f9a13)


#### Enable and start Node Exporter:

- sudo systemctl enable node_exporter
  
- sudo systemctl start node_exporter


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/0b72844d-295c-49d9-9dfa-975376ef09da)



Verify the Node Exporter's status:

- sudo systemctl status node_exporter


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/5e92f8b4-97ba-44db-aa20-269349929ac0)


- You can access Node Exporter metrics in Prometheus.


#### Configure Prometheus Plugin Integration:

Integrate Jenkins with Prometheus to monitor the CI/CD pipeline.

Prometheus Configuration:

To configure Prometheus to scrape metrics from Node Exporter and Jenkins, you need to modify the prometheus.yml file. Here is an example prometheus.yml configuration for your setup:

` global:
   
   scrape_interval: 15s

 scrape_configs:
     
     job_name: 'node_exporter'
     static_configs:
         
         targets: ['localhost:9100']

     job_name: 'jenkins'
     metrics_path: '/prometheus'
     
     static_configs:
         
         targets: ['<your-jenkins-ip>:<your-jenkins-port>'] `
   
    
- Make sure to replace <your-jenkins-ip> and <your-jenkins-port> with the appropriate values for your Jenkins setup.


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/b46bd180-1d2a-47f4-b5a8-2702c2a90135)


- Check the validity of the configuration file:

- promtool check config /etc/prometheus/prometheus.yml


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/067fb32e-b725-44a4-9c1b-d70f2926d681)


- Reload the Prometheus configuration without restarting:

- curl -X POST http://localhost:9090/-/reload

You can access Prometheus targets at:

http://<your-prometheus-ip>:9090/targets

![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/25ff039d-753b-4898-93aa-ecdf576a6174)


####Grafana

- Install Grafana on Ubuntu 22.04 and Set it up to Work with Prometheus

#### Step 1: Install Dependencies:

- First, ensure that all necessary dependencies are installed:

` sudo apt-get update
 sudo apt-get install -y apt-transport-https software-properties-common `


#### Step 2: Add the GPG Key:

- Add the GPG key for Grafana:

` wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add - `


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/005243f5-bd30-4170-88e7-a30891b396a3)


#### Step 3: Add Grafana Repository:

- Add the repository for Grafana stable releases:

` echo "deb https://packages.grafana.com/oss/deb stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list `


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/356f6185-c9fc-44a1-8a24-bf8e86b0c386)


#### Step 4: Update and Install Grafana:

- Update the package list and install Grafana:

` sudo apt-get update
  sudo apt-get -y install grafana `

#### Step 5: Enable and Start Grafana Service:

- To automatically start Grafana after a reboot, enable the service:

` sudo systemctl enable grafana-server `

- Then, start Grafana:

` sudo systemctl start grafana-server `

#### Step 6: Check Grafana Status:

- Verify the status of the Grafana service to ensure it's running correctly:

` sudo systemctl status grafana-server `


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/f11c84c1-3680-4af0-a2f5-ccfcacc50608)


#### Step 7: Access Grafana Web Interface:

 Open a web browser and navigate to Grafana using your server's IP address. The default port for Grafana is 3000. For example:

 http://<your-server-ip>:3000

 You'll be prompted to log in to Grafana. The default username is "admin," and the default password is also "admin."

#### Step 8: Change the Default Password:

When you log in for the first time, Grafana will prompt you to change the default password for security reasons. Follow the prompts to set a new password.


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/362e4db6-eb65-4648-91be-f1bc5547e548)


#### Step 9: Add Prometheus Data Source:

To visualize metrics, you need to add a data source. Follow these steps:

- Click on the gear icon (⚙️) in the left sidebar to open the "Configuration" menu.

- Select "Data Sources."

- Click on the "Add data source" button.

- Choose "Prometheus" as the data source type.

In the "HTTP" section:
Set the "URL" to http://localhost:9090 (assuming Prometheus is running on the same server).
Click the "Save & Test" button to ensure the data source is working.


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/de899b78-d7f3-4a67-b155-3574c8a43788)


#### Step 10: Import a Dashboard:

To make it easier to view metrics, you can import a pre-configured dashboard. Follow these steps:

- Click on the "+" (plus) icon in the left sidebar to open the "Create" menu.

- Select "Dashboard."

- Click on the "Import" dashboard option.

- Enter the dashboard code you want to import (e.g., code 1860).


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/cce4b973-f331-4491-a436-034d121b277a)


- Click the "Load" button.

- Select the data source you added (Prometheus) from the dropdown.

- Click on the "Import" button.

You should now have a Grafana dashboard set up to visualize metrics from Prometheus.


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/2b5a227e-3890-4df0-bf2b-cb9cd3ae0a08)


Grafana is a powerful tool for creating visualizations and dashboards, and you can further customize it to suit your specific monitoring needs.
That's it! You've successfully installed and set up Grafana to work with Prometheus for monitoring and visualization.

Configure Prometheus Plugin Integration:
Integrate Jenkins with Prometheus to monitor the CI/CD pipeline.

- install prometheus in jenkins > manage jenkins > plugins > available plugins 
  you will need to restart jenkins for the plugin to install successfully


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/131dfdc3-6254-4109-884d-46f4d303eed6)


- update the prometheus.yaml file and save .


![Screenshot 2023-12-23 121844](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/e7e7dd12-c2b5-4583-a2bf-6f982a60b104)


refresh to see jenkins status <up>


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/5017231a-5154-4894-88f8-a270d4c8e9f2)


- Click on the "+" (plus) icon in the left sidebar to open the "Create" menu.

- Select "Dashboard."

- Click on the "Import" dashboard option.

- Enter the dashboard code you want to import (e.g., code 9964).


![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/bd3dbfe4-9aa7-4b5d-bdce-329ceb7fc065)


- dashboard

![](https://github.com/UzonduEgbombah/Netflix-Clone-Project/assets/137091610/db2ba385-8e75-4b94-bca3-af0b1944b2ab)



