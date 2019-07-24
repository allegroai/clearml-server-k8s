# Allegro.ai trains server Helm chart
#### Pre-requirements
* You have a kubernetes cluster
* You have kubectl installed and configured [(install kubectl)](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
* You have helm installed [(install helm)](https://helm.sh/docs/using_helm/#installing-helm)
* You have node labeled 'app: trains' (see Additional Information)
#### Add trains server repository to your helm:
```sh
helm repo add allegroai https://allegroai.github.io/trains-helm/
```

Make sure repository was added correctly:
```sh
helm search trains
```
You should see allegroai/trains-server-chart

#### Installing trains server chat on your cluster
According to your search results, following command should deploy trains server on your cluster
```sh
helm install allegroai/trains-server-chart
```

This will create 'trains' namespace in your cluster and deploy everything in in it.

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
change this when you install chart using --set flag. In values.yaml you can view all configurations you can 
change with set. If for example you have one node, you can remove 
requirement for running a deployment on a tagged node with this command:
```sh
helm install allegroai/trains-server-chart --set trains.nodeSelector=""
```

