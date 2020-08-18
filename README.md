# odo

odo is a CLI tool for creating applications on OpenShift Container Platform. With odo, you can write, build, and debug applications on a cluster without the need to administer the cluster itself. Creating deployment configurations, build configurations, service routes and other OpenShift Container Platform or Kubernetes elements are all automated by odo.

Existing tools such as oc are operations-focused and require a deep understanding of Kubernetes and OpenShift Container Platform concepts. 
odo abstracts away complex Kubernetes and OpenShift Container Platform concepts allowing developers to focus on what is most important to them: code. Odo focuses on improving developer experience. 

1. Install `odo` cli 
    For macOS - 
    ```
    curl -L https://mirror.openshift.com/pub/openshift-v4/clients/odo/latest/odo-darwin-amd64 -o /usr/local/bin/odo
    chmod +x /usr/local/bin/odo
    ```
    Other OS follow this doc - https://access.redhat.com/documentation/en-us/openshift_container_platform/4.5/html/cli_tools/developer-cli-odo#installing-odo

2. Log in to the Openshift cluster and select the project you want to deploy your application. 
    `oc project <project name>`
    
3. Download the example Node.js application
   `git clone https://github.com/openshift/nodejs-ex`
   
4. Add a component of the type Node.js to your application: 
    ```
      cd nodejs-ex
      odo create nodejs
    ```
    
    **NOTE** By default, the latest image is used. You can also explicitly specify an image version by using odo create openshift/nodejs:8

5. Push the initial source code to the component :
    
   `odo push`
   
6. To verify that your component is deployed to OpenShift Container Platform :

   ```
      oc get deployments 
      oc get pods 
      oc get svc
   ```
7. Create a URL and add an entry in the local configuration file as follows. You must know your ingress domain name or ingress IP to specify --host for odo url create.

   `odo url create --port 8080`

## Devfile

1. Listing all available devfile components 
   `odo catalog list components`
   
2. Let's deploy a springboot application. Download the example repo. 
   `git clone https://github.com/odo-devfiles/springboot-ex`
   
3. Create a component configuration using the java-spring-boot component-type named myspring
   `odo create java-spring-boot myspring`

4. Deploy the application to openshift project 
   `odo push`
 
5. Verify the resources that got created 
    ```
       oc get deployments 
       oc get pods 
       oc get svc
    ```
6. Create a URL in order to access the deployed component :
   `odo url create`
   
   If you get the below message try the command by passing the ingress domain name via --host flag.
 
   `odo url create --host <hostname>`
   
   Eg: `odo url create --host ocp44.us.appdomain.cloud`
   
7. Push change to the cluster 
    `odo push`

8. List the URLs of the component:
   `odo url list`

9. Verify the url using a browser or curl command 
   `curl <URL>`

10. To delete the application :
    `odo delete`
    
## Creating a Java applications using devfiles

**NOTE** Technology Preview feature only. Don't use in production. 
1. Log in to the Openshift cluster and select the project you want to deploy your application. 
    `oc project <project name>`
2. Create a directory to store the source code of your component: 
    `mkdir <directory-name>`
3. Create a component configuration of Spring Boot component type named myspring and download its sample project: 

    `odo create java-spring-boot myspring --downloadSource`
   
   The odo create command downloads the associated devfile.yaml file from the recorded devfile registries. 
4. Create a URL to access the deployed component: 
    `odo url create --host <hostname>`
    eg: `odo url create --host cpat-dev.us-east.containers.appdomain.cloud`
    
5. Push the component to the cluster: 
    `odo push`
    
6. Verify the resources that got created:
    ```
       oc get deployments 
       oc get pods 
       oc get svc 
       oc get routes 
    ```   
7. List the URLs of the component to verify that the component was pushed successfully: 
    `odo url list`
    
8.  View your deployed application by using the generated URL:
    `curl http://myspring-8080.<domain-name>`
