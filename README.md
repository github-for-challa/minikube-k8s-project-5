# minikube-k8s-project-5
 TASK 5: Build a Kubernetes Cluster Locally with Minikube  Objective: Deploy and manage apps in Kubernetes
Sure! Here's a README file that you can use to save the step-by-step instructions in your GitHub repository.

markdown
Copy
Edit
# Kubernetes Deployment with Minikube

This guide outlines the steps for deploying and managing applications in a Kubernetes cluster using Minikube. It includes creating a deployment for a simple app (Nginx), exposing it with a service, and verifying the setup.

## Steps for Deployment and Exposure

### 1. **Install Minikube**
   To begin, ensure Minikube is installed. If not, follow the installation steps from the official Minikube documentation for your OS.

   After installation, start the Minikube cluster:
   minikube start

# 2. Create deployment.yaml for App Deployment
Create a deployment YAML file that defines your app's deployment in the Kubernetes cluster. This file specifies the number of replicas, container image, and ports for your app.

Example deployment.yml:

apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app-deployment
  labels:
    app: my-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app
        image: docker.io/docker_username/nginx:latest
        ports:
        - containerPort: 80

# 3. Create service.yaml to Expose the App
To expose your app outside the Kubernetes cluster, define a Service of type NodePort. This service will make your app accessible via a specific port on the host machine.

Example service.yml:

apiVersion: v1
kind: Service
metadata:
  name: my-app-service
spec:
  type: NodePort
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 3000
      nodePort: 30081

# 4. Apply the Deployment and Service
Apply the deployment.yml and service.yml files to your Kubernetes cluster.
kubectl apply -f deployment.yml
kubectl apply -f service.yml

# 5. Verify the Deployment
Check the status of the deployment and the pods running in the cluster using the following commands:

kubectl get pods
kubectl get svc my-app-service

# 6. Scale the Deployment
If you want to scale the app, use the following command to change the number of replicas:
kubectl scale deployment my-app-deployment --replicas=5

# 7. Verify the App's Logs and Status
To inspect the app's logs and confirm it's running correctly, use:
kubectl describe pod <pod-name>

# 8. Access the Application
After deploying and exposing the app, you can access it through the NodePort. Use the following command to get the URL:
## minikube service my-app-service --url
This will output a URL like http://127.0.0.1:43309, where you can access your app.

# 9. Scaling and Additional Checks
To scale the application, use the command:
kubectl scale deployment my-app-deployment --replicas=5
You can also describe the pod for more detailed logs or troubleshooting.

# Conclusion
By following these steps, you have successfully created a Kubernetes deployment using Minikube, exposed it via a NodePort service, and scaled the deployment. You can now access the deployed app through Minikube's service URL and verify its logs to ensure it's running correctly.
Note: If you are using Docker Desktop or a similar tool, make sure to verify that the app is running on the correct ports and accessible via the local network. If you're running Minikube on Linux, the minikube service command will help you open the app in a browser.
