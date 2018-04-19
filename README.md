# K8Lab

This lab is to kick start your journey IBM Container Service, You can find instructions to execute and supporting files in this repo

# Setting up environment
Set up IBM Cloud CLI : Command line interface to manage applications, containers, infrastructures, services
https://console.bluemix.net/docs/cli/reference/bluemix_cli/get_started.html#getting-started
verify by using command like bx help

Install IBM cloud CLI(bx) :
```
https://console.bluemix.net/docs/cli/reference/bluemix_cli/get_started.html#getting-started
```

Install container service  Plugins : 
```
bx plugin install container-service -r Bluemix
```

Create Cluster on IBM cloud: Login with ibm cloud credentionals and create service Containers in Kubernetes Clusters from service catalog. Please refer below link.
https://console.bluemix.net/docs/containers/container_index.html#container_index

Install Kubernetes Cluster :
```
https://console.bluemix.net/containers-kubernetes/catalog/cluster/create
```

Install and Set Up kubectl: kubectl, a Kubernetes command-line tool, to deploy and manage applications on Kubernetes.
There are multiple to option to install kubectl.

Install Kubernetes CLI:
```
https://kubernetes.io/docs/tasks/tools/install-kubectl/
```

Install Helm: 
```
https://github.com/kubernetes/helm/blob/master/docs/install.md
```

# Steps-Kubernetes Resources:

1. Gain access to your cluster
   Log in to your IBM Cloud account. I have created cluster in sydney region(au-syd)
```   
   bx login -a https://api.au-syd.bluemix.net
   bx cs region-set ap-south
```

2. Set the context for the cluster in in your CLI.
   Get the command to set the environment variable and download the Kubernetes configuration files.
```   
   bx cs cluster-config <Your_Cluster_Name>
```

3. Set the KUBECONFIG environment variable. Copy the output from the previous command and paste it in your terminal. The command output should look similar to the following.

```
   export KUBECONFIG=/Users/ibm/.bluemix/plugins/container-service/clusters/<your_cluster_name>/kube-config-mel01-your_cluster_name.yml
```
 
4. Verify that you can connect to your cluster by listing your worker nodes.
```
   kubectl get nodes
```

5. Download the nginx.yaml and Run below command. 
```
   kubectl create -f nginx.yaml
```

6. bx cs workers <your_cluster_name_created_under_ibm_cloud>
    The output will be similar to as below. 
    The public ip will be your cluster ip to access your applcation
``` 
    bx cs workers <your_cluster_name>
    ID                                                  Public IP        Private IP
    kube-mel01-paedbc7786e21c450e813eadc69ebaf43b-w1    168.1.149.16     10.118.243.226
``` 

7. Get the port exposed by service using node-port

``` 
   $ kubectl get svc
   NAME             CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
   kubernetes       172.21.0.1       <none>        443/TCP        8d
   nginxservice-4   172.21.231.240   <nodes>       80:30090/TCP   1d

``` 

 
7. You can access nginx application by below url.
```
    http://<your_cluster_ip>:30090
```    


# Steps-Helm:

1. Install Helm Client 
        https://docs.helm.sh/using_helm/#installing-helm
        From Homebrew (macOS)
        Members of the Kubernetes community have contributed a Helm formula build to Homebrew. This formula is generally up to date.
      ```
        brew install kubernetes-helm 
      ```
        Based on your OS install helm client

2. Login to IBM cloud, Execute “bx login “on your local system terminal window to login. Note if you are using IBM federated email then execute with –sso option.
 
3. After successful login to IBM cloud, Initialize the Container plugin by executing the below command.
Execute  ``` bx cs init ```
 
4. Make sure kubectl is installed properly and it can communicate to the cluster.

   ``` kubectl version  ```  
   the output of this command should return both server and client version, you can see the client and server version in the below sample output.
   Example output:
```
   Client Version: version.Info{Major:"1", Minor:"7", GitVersion:"v1.7.3",    GitCommit:"2c2fe6e8278a5db2d15a013987b53968c743f2a1", GitTreeState:"clean", BuildDate:"2017-08-03T07:00:21Z", GoVersion:"go1.8.3", Compiler:"gc", Platform:"darwin/amd64"}
   Server Version: version.Info{Major:"1", Minor:"8+", GitVersion:"v1.8.8-2+9d6e0610086578", GitCommit:"9d6e06100865789613cbac936edce948f0710a2f", GitTreeState:"clean", BuildDate:"2018-02-23T08:20:09Z", GoVersion:"go1.8.3", Compiler:"gc", Platform:"linux/amd64"}
```

5. This step is required only if kubectl client is not able to connect to your cluster or if server version is not returned.
 
6. Execute ```bx cs cluster``` to get the list of clusters available in your account. If there is no cluster reported then create one by login to Bluemix console.
 
7. Execute ```bx cs cluster-config <cluster name>```, 
   
   Replace <cluster name >  with your cluster name, received from step 4.1, Output of this command return the cluster name and environment setting need to be execute.
   
   Execute the export command in the output of cluster-config command.
   Example: ``` export KUBECONFIG=/Users/rameshpoomalai/.bluemix/plugins/container-service/clusters/mycluster/kube-config-hou02-mycluster.yml ```

8. Execute the following command to package your charts.
```helm package letschat```
   This will package your charts and that be released to release to your repos.
 
9. Execute ```helm install --debug --dry-run letschat --name frontend``` to to test your charts
 
10. Execute ```helm install letschat --name frontend``` to install your package on your cluster.

11. Execute the following command to package your charts.
   ```helm package mongo```
   This will package your charts and that be released to release to your repos.
 
12. Execute ```helm install --debug --dry-run mongo --name bankend``` to to test your charts
 
13. Execute ```helm install mongo --name bankend``` to install your package on your cluster.



