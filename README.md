<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Deploy Backend with Kubernetes

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-compute-eks4)

**Author:** Vijay Pratap Singh Hada  
**Email:** vijaypratapsinghhada9@gmail.com

---

## Deploy Backend with Kubernetes

![Image](http://learn.nextwork.org/blissful_yellow_calm_donkey/uploads/aws-compute-eks4_6cfb382f2)

---

## Introducing Today's Project!

In this project, I will deploy backend  of appplication for development and also going to learn about `kubectl` and going to delpoy the backend for k8 cluster and also going to track k8 using EKS service.

### Tools and concepts

I used Kubernetes, ECR, kubectl, and Docker to deploy a backend application. Key concepts include using manifests to define the deployment and service, building a container image with Docker, storing the image in ECR, and using kubectl to interact with the Kubernetes cluster.

### Project reflection

This project took me approximately 30 minutes for the hands-on work of setting up and deploying, and an additional 35 minutes for writing the documentation.

---

## Project Set Up

### Kubernetes cluster

To set up today's project, I launched a Kubernetes cluster. The cluster's role in this deployment is to be the main place where my application runs and is managed. I created this cluster using Amazon EKS, which makes setting up Kubernetes on AWS simpler. I launched a virtual machine (an EC2 instance) and installed a tool called `eksctl` on it. I also gave the EC2 instance permission to work with other AWS services by attaching an IAM role. Then, using `eksctl`, I created the EKS cluster with specific settings, which will now handle running and keeping track of my application's containers.

### Backend code

I retrieved the backend code by cloning a GitHub repository. Pulling code is essential to this deployment because I need the actual application files to build a container image and then deploy it onto my Kubernetes cluster. I used the Git command-line tool, which I installed on my EC2 instance, to copy the entire repository containing the backend code to my working environment. This gives me all the necessary files, like the application code and configuration files, to move forward with packaging and deploying the backend.

### Container image

Once I cloned the backend code, I built a container image because Kubernetes needs a standardized package of the application to deploy and manage it consistently. Without an image, it would be difficult for Kubernetes to reliably create multiple instances of the application with all its dependencies included. I used Docker to build this image from the code and a Dockerfile, ensuring everything the backend needs is bundled together for Kubernetes to use as a blueprint. I also configured my user to run Docker commands without `sudo` for easier workflow.

I also pushed the container image to a container registry, which is a storage location for container images. ECR facilitates scaling for my deployment because it provides a central, accessible place for my Kubernetes cluster to pull the image from. 

---

## Manifest files

Kubernetes manifests are instruction files that tell Kubernetes how to deploy and manage your application. Manifests are helpful because they act as a blueprint for your application's desired state, detailing things like which container images to use, how many copies of your application to run, and how to expose it to the outside world. 

A Deployment manifest manages how Kubernetes runs and updates multiple copies of your application. The container image URL in my Deployment manifest tells Kubernetes exactly where to find the container image it needs to use when creating instances of my backend application. 

A Service resource exposes your application running inside Kubernetes to network traffic, either from within the cluster or from external sources. My Service manifest sets up a way for external traffic to reach my backend application by routing it to the correct containers within the cluster. It defines which containers receive traffic and on which ports, essentially acting as a traffic manager for my deployed application so users or other services can access it.

![Image](http://learn.nextwork.org/blissful_yellow_calm_donkey/uploads/aws-compute-eks4_b01876554)

---

## Backend Deployment!

To deploy my backend application, I installed `kubectl`, the command-line tool for interacting with Kubernetes. After confirming `kubectl` was installed correctly, I used it to apply my deployment and service manifest files. This action tells Kubernetes to create the necessary resources within the cluster based on the instructions in those files, effectively deploying my application.

### kubectl

kubectl is a command-line tool for controlling your Kubernetes cluster. I need this tool to deploy my backend application and manage things like the running copies of my app. I can't use eksctl for the job because eksctl is mainly for setting up and taking down the EKS cluster itself, not for putting applications onto the cluster once it's ready. Kubectl is specifically designed for interacting with the applications and resources inside the cluster.

![Image](http://learn.nextwork.org/blissful_yellow_calm_donkey/uploads/aws-compute-eks4_6cfb382f2)

---

## Verifying Deployment

My extension for this project is to use the EKS console to check on my deployed application and its resources. I had to set up IAM access policies because, even with AWS administrative permissions, Kubernetes has its own separate system for managing who can access what within the cluster. I set up access by using the `eksctl` command-line tool to create an IAM identity mapping, linking my AWS user to a group within Kubernetes that has the necessary permissions to view the cluster's nodes and deployment details.

Once I gained access into my cluster's nodes, I discovered pods running inside each node. Pods are the smallest units that can be deployed in Kubernetes, acting as bundles for one or more containers that work together. Containers in a pod share the same network and storage resources, allowing them to easily communicate and share data, which is essential for applications made up of multiple cooperating containers.

The EKS console shows you the events for each pod, where I could see the steps Kubernetes took to create and run my backend application's pod, including pulling the image and starting the container. This validated that the container image was successfully fetched from ECR and the backend application is now running within a pod inside the cluster. Seeing these events confirmed that my deployment process worked as expected and the backend is active and assigned an internal IP address.

![Image](http://learn.nextwork.org/blissful_yellow_calm_donkey/uploads/aws-compute-eks4_3b391f873)

---

---
