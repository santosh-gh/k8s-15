# Step 1: Start Minikube

    First, start Minikube. 

    minikube start    

# Step 2: Create a Namespace for Argo CD

    Create a separate namespace for Argo CD.

    kubectl create namespace argocd
    
# Step 3: Install Argo CD
    
    Install Argo CD

    kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

    kubectl get all -n argocd

# Step 4: Expose the Argo CD Server

    Get the service URL:

    minikube service argocd-server -n argocd --url

    To access the Argo CD web interface, expose the Argo CD server using the kubectl port-forward command:

    kubectl port-forward svc/argocd-server -n argocd 8080:443

    This command forwards the port 8080 on the local machine to port 443 of the Argo CD server.

# Step 5: Log in to Argo CD

    Browse to https://localhost:8080.

    Retrieve the password using the following command:

    kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 -d; echo

    The output will be the name of the Argo CD server pod. Use this name as the password. The default username is admin.

# Step 6: Connect a Git Repository

    Connect a Git repository to Argo CD. This repository should contain the Kubernetes manifests for the applications you want to deploy.

    In the Argo CD web interface, click on New App.
    Fill in the details like Application Name, Project, Sync Policy, and the Git repository URL.
    Specify the path within the repository where the manifests are stored.
    Choose the destination cluster and namespace.
    Click on Create to create the application.

# Step 7: Sync the Application

    After creating the application, you will see it listed on the dashboard. 
    Click on the Sync button to deploy the application. 
    Argo CD will pull the manifests from the Git repository and apply them to the Minikube cluster.