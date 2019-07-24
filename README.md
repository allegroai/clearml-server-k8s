# Allegro.ai trains server on Kubernetes
#### Pre-requirements
* You have a kubernetes cluster
* You have kubectl installed and configured [(install kubectl)](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
* You have node labeled 'app: trains' (see Additional Information)
#### Clone trains server Kubernetes repository
```sh
git clone <allegro ai kubernetes repository clone url>
```

Go the cloned folder
```sh
cd <clonned repository>
```

#### Deploying to Kubernetes cluster
If this is your first time running this deployment, you should create trains namespace:
```sh
kubectl create -f trains-namespace.yaml 
```
All deployment will be on trains namespace.

Run following command to deploy trains server services on your cluster:
```sh
kubectl apply -f services.yaml
```

Run following command to create all the deployments:

```sh
kubectl create -f elasticsearch-deployment.yaml,mongo-deployment.yaml,apiserver-deployment.yaml,fileserver-deployment.yaml,webserver-deployment.yaml
```

#### Ports mapping
Services expose following node ports:<br>
app: 30080<br>
api: 30008<br>
files: 30081<br>

30080 maps to trains webserver container on port 8080<br>
30008 maps to trains apiserver container on port 8008<br>
30081 maps to trains fileserver container on port 8081<br>

#### Accessing trains-server
Indented way to use trains server is to create a load balancer and domain name with records pointing to it.
Trains translates domain names in a specific way. This are the rules you need to follow for it to work. <br>

Record to access web page:
* \*app.\<your domain name\>.* (example: trainsapp.mydomainname.com) should point to your node on port 30080

Record to access api:
* \*api.\<your domain name\>.* (example: trainsapi.mydomainname.com) should point to your node on port 30008

Record to access files:
* \*files.\<your domain name\>.* (example: trainsfiles.mydomainname.com) should point to your node on port 30081

Note: by editing services.yaml you can reconfigure ports your node listens to
#### Additional Information
* By default, deployment is looking for node tagged as 'app: trains' but you can  
change this by editing deployment yaml's nodeSelectors.
