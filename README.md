# RAG Demo

These are the steps to running the [Redis Vector Store tutorial](https://docs.llamaindex.ai/en/stable/examples/vector_stores/RedisIndexDemo.html) from LlamaIndex.

In order to complete this tutorial you will need: OpenShift, access to at least 1 GPU, and Red Hat OpenShift AI. The Huggingface TGI server alone requires 8 cpu cores and 20Gi of memory, so ensure your environment has enough resources before you begin.

## Setting up the environment

You will need to have your GPU enabled at this point. Download this github locally and go to the directory in your terminal. You will also need to set up the Red Hat OpenShift AI Operator. To do so, go to the OperatorHub and search for "Red Hat OpenShift AI" and click install. Once you install, you will have to create an instance of the Data Science Cluster and leave all defaults. Next, you will need to create a new project for this tutorial so run the following command: 
```
oc new-project llama-example
```

## Deploy Huggingface TGI Server

To deploy the server, you will need to go to the huggingface-tgi directory. Once you are in the directory, you're going to create the initial deployment first, then a service.
``` 
oc create -f deployment.yaml
```
Now we'll deploy the service and expose it for future access with the following commands:
```
oc create -f service.yaml
oc expose service tgis-service
```

## Deploy Redis Stack

Now we want to deploy redis stack to use as our database. We are going to deploy this image to create our redis-stack with this command:
```
oc new-app redis/redis-stack
```
Next we have to add a load balancer to make our redis stack externally accessible. Go to the redis-stack directory and then run this:
	Use yaml from redis-loadbalancer.yaml
```
oc create -f redis-loadbalancer.yaml
```
After you create the load balancer, add the following labels: 
```
app.kubernetes.io/instance=redis-stack
app.kubernetes.io/component=redis-stack
app=redis-stack
```
And add this to pod selector: 
```
deployment=redis-stack
```

## Start RHOAI Environment with Jupyter

First you want to create a new Data Science Project and label it llama-project. Then you want to create a workbench, where you'll set the image size to large and select your GPU. Once your workbench is finished deploying, launch it and create a new notebook file. Next you'll walk through the llamaindex [Redis Vector Store example](https://docs.llamaindex.ai/en/stable/examples/vector_stores/RedisIndexDemo.html). Go through the notebook and when you get to creating your database in your redis stack, get the external load balancer address from Openshift for the stack and replace the URL in the notebook to look like this: redis://<LOAD_BALANCER_ADDRESS>:6379. It is important to use port 6379 as we exposed that port specifically when we created the load balancer.

