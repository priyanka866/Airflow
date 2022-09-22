# Airflow
Airflow Setup in kubernetes 
source-https://marclamberti.com/blog/airflow-on-kubernetes-get-started-in-10-mins/
https://www.astronomer.io/events/recaps/official-airflow-helm-chart/
Helm chart-https://www.notion.so/Airflow-Helm-Chart-Quick-start-for-Beginners-3e8ee61c8e234a0fb775a07f38a0a8d4

Prerequisites
1) Kubcetl
2) Helm
3) Kind
4) Docker

# Create a kubernetes cluster of 1 control plane and 3 worker nodes
Create a folder and add kind-cluster.yml
kind create cluster --name airflow-cluster --config kind-cluster.yaml

# Check the cluster info
kubectl cluster-info

# Check the nodes with kubectl
kubectl get nodes -o wide

# Add the official repository of the Airflow Helm Chart
helm repo add apache-airflow https://airflow.apache.org

# Update the repo
helm repo update

# Create namespace airflow
kubectl create namespace airflow

# Check the namespace 
kubectl get namespaces

# Install the Airflow Helm Chart
helm search repo airflow
helm install airflow apache-airflow/airflow --namespace airflow --debug
helm show values apache-airflow/airflow > values.yaml
Update excecutor to KubernetesExecutor in values.yml
eg= executor: "KubernetesExecutor"

# Get pods
kubectl get pods -n airflow

# Check release
helm ls -n airflow
If for some reasons the release is stuck in pending-install or timed out

# Resinstall the chart
helm delete airflow --namespace airflow
helm install airflow apache-airflow/airflow --namespace airflow --debug --timeout 10m0s
Note-it will be [--timeout] unlike what mentioned in the source. 

# Port forward 8080:8080
kubectl port-forward svc/airflow-webserver 8080:8080 -n airflow --context kind-airflow-cluster
1. View pods
kubectl get pods -n airflow
2. Log pods 
kubectl logs <<Pod_name>> -n airflow -c scheduler  (-c is used to specify which container)

#Airflow git sync
Create a ssh key and add into the git repository
