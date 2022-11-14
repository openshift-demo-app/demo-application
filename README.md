# demo-application

1.	Creating a project
A project allows a community of users to organize and manage their content in isolation from other communities.
Follow the below steps to create a project in Openshift cluster using CLI
Step 1. Login to the Openshift cluster and copy the token for login.
 

Step 2. Create a new project using the command below

oc new-project demo-project --description="This is an example project for openshift demo" --display-name="Openshift project for demo"

Step 3. List the project in the cluster using the command below

oc get project

Step 4. Change from the current project to the newly created project for CLI operations. The specified project is then used in all subsequent operations that manipulate project-scoped content

oc project demo-project

2.	Creating an application
To create an application, we will use the project which we have created in the previous step.
The code used for the application is hosted in the git repository. GIT helps in managing and maintain the code.

oc new-app https://github.com/openshift-demo-app/demo-application -l name=demo-app
    
Above command will do the below tasks:
It will check out the com

To trigger build manually, use the below command

oc start-build nodejs-ex --follow

3.	Accessing the application

Get the service details to access the application within the cluster using the below command:
oc get svc
Example output (if you get multiple services, then check for your application name, in this case “demo-application”)

To expose the application, you need to create a route using the below command:
oc expose svc/demo-application

Check if route is created for the service using the below command:
oc get routes
4.	Configuring Node JS application with a database

4.1 Create another app to deploy mongo DB using the command below:
oc new-app centos/mongodb-26-centos7 -e MONGODB_USER=admin -e MONGODB_DATABASE=mongo_db -e MONGODB_PASSWORD=secret -e MONGODB_ADMIN_PASSWORD=super-secret
 4.2 Check the status of the deployed application using the command below:
oc get status
4.3 Update the node js application to use the newly created database

oc set env deployment/demo-application MONGO_URL=mongodb://admin:secret@ 172.30.127.119:27017/mongo_db
