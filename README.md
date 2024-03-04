# RAG Demo

These are the steps to running the [Redis Vector Store tutorial](https://docs.llamaindex.ai/en/stable/examples/vector_stores/RedisIndexDemo.html) from LlamaIndex.

In order to complete this tutorial you will need: OpenShift 4.14, access to at least 1 GPU, and Red Hat OpenShift AI. 

## Setting up the environment

You will need to have your GPU enabled at this point. Download this github locally and go to the directory in your terminal. You will also need to set up the Red Hat OpenShift AI Operator. To do so, go to the OperatorHub and search for "Red Hat OpenShift AI" and click install. Once you install, you will have to create an instance of the Data Science Cluster and leave all defaults. Next, you will need to create a new project for this tutorial so run the following command: 
```
oc new-project llama-example
```

## Deploy Redis Stack

Now we want to deploy redis stack to use as our database. For this example, we'll pull the latest version of redis stack. We are going to deploy this image to create our redis-stack with this command:
```
oc new-app redis/redis-stack
```
Next we want to add a volume to the redis stack. To do this, we are going to make some changes to our redis stack deployment with the update.yaml file in the *redis-stack* directory. Once you are in the *redis-stack* directory, run the fllowing command.
```
oc apply -f update.yaml
```
Now that we've set up the volumes, we're ready to head over to RHOAI. 

## Start RHOAI Environment with Jupyter

Once you are at the RHOAI dashboard, head over to the Data Science Projects tab. You want to create a new Data Science Project and label it llama-project. Then you want to create a workbench, where you'll select the PyTorch image, set the image size to large, and select your GPU. Once your workbench is finished deploying, launch it and create a new notebook file. </br>
</br>
Next you'll import this repository into our RHOAI environment. On the left hand side, select the git icon and select *Download a Repository*. Paste the link from this repo to import it. </br>
</br>
Its time for the tutorial! Walk through the RedisIndexDemo.ipynb to see how we store vectors in a redis stack with llamaindex.

