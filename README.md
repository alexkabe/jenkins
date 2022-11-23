## Jenkins

Installation of jenkins on kubernetes

## Command line

## 1- The Namespace

First we need to create the namespace on kubernetes `kubectl create namespace "namespace_name"`

## 2- Configure Helm 

Add the jenkinsci on the helm `helm repo add jenkinsci https://charts.jenkins.io` after that you need to update helm repo throught the command `helm repo update` 

## 3- PVC

Create a pvc for jenkins. Get the yaml file and after that, execute this command to create a pvc. `kubectl apply -f jenkins-volume.yaml`

## 4- Service Account 

To create the service account, execute the command `kubectl apply -f jenkins-sa.yaml` after create and complete the yaml file.

## 5- Helm install

Create the jenkins with the command `helm install jenkins -n jenkins  jenkinsci/jenkins` after that we need to get the password to connect on jenkins.
- `$ jsonpath="{.data.jenkins-admin-password}"`
- `$ secret=$(kubectl get secret -n jenkins jenkins -o jsonpath=$jsonpath)`
- `$ echo $(echo $secret | base64 --decode)`

To get the url of jenkins we need to type this commad

- `$ jsonpath="{.spec.ports[0].nodePort}"`
- `$ NODE_PORT=$(kubectl get -n jenkins -o jsonpath=$jsonpath services jenkins)`
- `$ jsonpath="{.items[0].status.addresses[0].address}"`
- `$ NODE_IP=$(kubectl get nodes -n jenkins -o jsonpath=$jsonpath)`
- `$ echo http://$NODE_IP:$NODE_PORT/login`

## Further help

To get more help on the Installation of Jenkins go to Jenkins (https://www.jenkins.io/doc/book/installing/kubernetes/) page.