# AWS EKS
## 📦 Demo 5
This project is part of **Module 11: Kubernetes on AWS (EKS)** in the **TWN DevOps Bootcamp**. The goal of this demo is to implement CD by automatically deploying a containerized application to an **DOKS** using  **Jenkins pipeline**.

[GitLab Repo](https://gitlab.com/devopsbootcamp4095512/devopsbootcamp_8_jenkins_pipeline/-/tree/deploy-to-digitalocean-ke?ref_type=heads)

## 📌 Objective
Deploy DOKS cluster from Jenkins pipeline

## 🚀 Technologies Used

- **kubectl**: CLI to interact with Kubernetes.
- **eksctl**: CLI tool for creating and managing EKS clusters
- **DigitalOcean**: hosting Jenkins server and Managed Kubernetes Cluster.
  
## 📋 Prerequisites
- Ensure you have an DigitalOcean Account.
- Kubectl is installed and configured to connect to the Kubernetes cluster.
- Jenkins server is running
- We are going to use the java-mave-app repository from previous demo.
  
## 🎯 Features
- Create K8 Cluster on DigitalOcean.
- Install kubectl as Jenkins Plugin.
- Update Jenkinsfile to use the Plugin and deploy DOKS
## 🏗 Project Architecture



## ⚙️ Project Configuration

### Creating DigitalOcean K8
## Creating a Managed K8 Cluster using DigitalOcean Kubernetes Engine.
1. Sign in to DigitalOcean and create a Kubernetes cluster.

   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_10_Kubernetes_DO_MongoDB/blob/main/Img/1%20creating%20k8%20digitalocean.png" width=800 />
   
2. Configure the cluster by specifying the node pool name, data center region, and cluster capacity
   
   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_11_AWS_EKS__CD_pipeline_DO/blob/main/Img/1%20create%20DO%20K8%20cluster.png" width=800 />
   
3. In the Overview section of the cluster configuration, download the cluster configuration file.
   
   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_11_AWS_EKS__CD_pipeline_DO/blob/main/Img/2%20download%20config%20file.PNG" width=800 />
   
4. Update the file permissions to read-only.

   ```bash
   chmod 400 k8s-1-32-2-do-3-nyc1-1749086389206-kubeconfig.yaml
   ```
   
5. Set the KUBECONFIG environment variable to the path of the configuration file.
    
   ```bash
   export KUBECONFIG=k8s-1-32-2-do-3-nyc1-1749086389206-kubeconfig.yaml  
   ```
   
   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_11_AWS_EKS__CD_pipeline_DO/blob/main/Img/3%20pointing%20kubectl%20to%20digitalocean%20cluster.PNG" width=800 />
   
6. Verify that the nodes are active

    ```bash
    kubectl get node
    ```
    <img src="" width=800 />


### Installing kubectl Jenkins Plugin
1. Install the AWS Credentials plugin:

2. Go to Manage Jenkins > Plugins > Available Plugins.

3. Search for Kubernetes CLI
   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_11_AWS_EKS__CD_pipeline_DO/blob/main/Img/5%20jenkins%20install%20k8%20CLI%20plugin.png" width=800 />

4. Click Install

### Adding K8 config yaml file to Jenkins
1. Go to Manage Jenkins.
2. Add New Global Credentials.
   * Kind: Secret File
   * Scope: Global
   * File: Choose the DigitalOcean K8 configuration file.
   * ID: Add an ID to the New Credentials
3. Save

<img src="https://github.com/lala-la-flaca/DevOpsBootcamp_11_AWS_EKS__CD_pipeline_DO/blob/main/Img/4%20create%20crentials%20for%20digital%20ocean%20importing%20the%20config%20file.png" width=800 />

### Updating Jenkins File
1. In your Java-Maven-App repository, create a new feature branch
   
3. Update the Jenkinsfile to authenticate using the kubectl plugin  in the deploy stage, ensure the credentialsId matches the Id from previous step.
   ```bash
   stage('deploy') {   
              steps {
                      script {   
                          echo 'deploying docker image...'
  
                          withKubeConfig([credentialsId: 'digital-ocean-ke-credentials']) {                                                    
  
                              sh 'kubectl get nodes'
                              sh 'kubectl create deployment nginx-deployment --image=nginx'
                          }
                      }
              }
          }

   ```
<img src="https://github.com/lala-la-flaca/DevOpsBootcamp_11_AWS_EKS__CD_pipeline_DO/blob/main/Img/6%20addiing%20authentication%20to%20pipeline.png" width=800 />

4. Run pipeline
<img src="https://github.com/lala-la-flaca/DevOpsBootcamp_11_AWS_EKS__CD_pipeline_DO/blob/main/Img/7%20pipeline%20ok.PNG" width=800 />

<img src="https://github.com/lala-la-flaca/DevOpsBootcamp_11_AWS_EKS__CD_pipeline_DO/blob/main/Img/8%20nginx%20pod%20running.PNG" width=800 />
