# AWS_EKS_Auto_Scaling
Auto Scaling in Amazon Elastic Kubernetes Service (EKS) typically involves two main components: the Kubernetes Horizontal Pod Autoscaler (HPA) for scaling pods and the Cluster Autoscaler for managing the underlying worker nodes. Here's the required code and steps for setting up AWS EKS Auto Scaling:

Steps:

Set Up an EKS Cluster:
We should first create an Amazon EKS cluster. You can use the AWS Management Console or AWS CLI to create the cluster. Make sure you have the AWS CLI and eksctl (EKS Command Line Utility) installed.

Use file: "Setup_EKS_cluster.txt" and execute

Configure kubectl:
To interact with the EKS cluster, configure kubectl:  
use file : "config_eks_with_kubectl"

We need to deploy our applications and configure Horizontal Pod Autoscalers. Here's an example Deployment and HPA YAML for a sample application:

apiVersion: apps/v1
kind: Deployment
metadata:
  name: sample-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: sample-app
  template:
    metadata:
      labels:
        app: sample-app
    spec:
      containers:
      - name: sample-app
        image: nginx


HPA: ( it does autoscale based on metrics values defined)
apiVersion: autoscaling/v2beta2
kind: HorizontalPodAutoscaler
metadata:
  name: sample-app-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: sample-app
  minReplicas: 1
  maxReplicas: 5
  metrics:
  - type: Resource
    resource:
      name: cpu
      targetAverageUtilization: 50


Cluster Autoscaler Configuration:

EKS enables Cluster Autoscaler by default, but you can configure it if needed. The cluster-autoscaler is responsible for scaling the worker node group based on resource requirements.

Use file: "cluster-autoscaler.yml"

Node Group Tags:
To enable the Cluster Autoscaler, you need to tag your Auto Scaling Group (ASG) for worker nodes. Add the tag k8s.io/cluster-autoscaler/enabled with the value true to your ASG. This allows the Cluster Autoscaler to discover and manage your node groups.
Additional AWS Resources:
Ensure that your AWS resources, such as the Launch Configuration, IAM roles, and networking, are properly set up to support auto scaling. Refer to the official AWS EKS documentation for details on setting up these resources.
Monitoring and Tuning:
Monitor your cluster's performance and tune the HPA and Cluster Autoscaler configurations based on your application's resource requirements and usage patterns






