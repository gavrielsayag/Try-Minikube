# Try-Minikube

## About

Trying to work with kubernetes, using MiniKube and KubeCTL in order to create a cluster and start working with it.

## Preperation

Make sure that you have *MiniKube* and *KubeCTL*.

## Creating a cluster

In order to create a cluster we need to tell *MiniKube* what driver to use. We can use *Docker* so that the image will be downloaded with all of the necessary things.

```ssh
minikube start --driver docker
```

![Minikube start](./images-for-readme/minikube_start.png)

## MYSQL

In this folder we have 3 configuration files (.yaml) that relate to MYSQL:

* ***mysql-config*** - a config map with the MYSQL URL.
* ***mysql-secret*** - a configuration file with secret values -  the password to the database.
* ***mysql.yaml*** - Deployment and Service.

## Employees

In this folder we have onlu one configuration file ***employees.yaml*** which have the Deployment and Service.<br>
In this part we are using the image of the employees project (the project is in my github and the image is in docker hub) - we can see that we give the name of the image in the *containers* section under the *Deployment* component.<br>
We are exposing the port *30000* using the *nodePort* field:
![Minikube exposing employees](./images-for-readme/exposing_employee_port.png)

## Deploy recources in MiniKube cluster
In order to do it we will use the *KubeCTL*.<br>
We need to start with the *ConfigMaps* and *Secrets* because that the *Deployments* and *Services* are referencing their value.<br>
Also we need to add the DB *Deployment* before the employees *Deployment* because the it relies on the DB.

In the *MYSQL* folder:
```ssh
kubectl apply -f mysql-config.yaml
kubectl apply -f mysql-secret.yaml
kubectl apply -f mysql.yaml
```
![Minikube deploy MYSQL](./images-for-readme/minikube_deploy_mysql.png)

In the *Employees* folder:
```ssh
kubectl apply -f employees.yaml
```
![Minikube deploy Employees](./images-for-readme/minikube_deploy_employees.png)

We can see the components using the following command
```ssh
kubectl get all
```
![KubeCTL get all](./images-for-readme/get_all.png)

* We can see our pods, we expect to see 4 pods - 3 of the *Employees* and one of *MYSQL* - the first section.
* Then we can see the services, 2 services that we created and one kebernetes service.
* Then we can see the *Deployments* - we created 2 deployments.

## Consume our Employees service
We can get the ip as an output to the command:
```ssh
minikube ip
```
So we can have the ip, and we set earlier the ***nodePort*** to *30000*.<br>
Now we can access the service easily.