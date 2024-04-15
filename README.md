# Deploy Machine Learning App built using Streamlit and PyCaret on Google Kubernetes Engine
#### A step-by-step beginner's guide to containerize and deploy streamlit app on Google Kubernetes Engine

Read the complete post: https://medium.com/@moez_62905/deploy-machine-learning-app-built-using-streamlit-and-pycaret-on-google-kubernetes-engine-fd7e393d99cb

- Official Website : https://www.pycaret.org

- Follow us on LinkedIn : https://www.linkedin.com/company/pycaret/

- Subscribe to our YouTube : https://www.youtube.com/channel/UCxA1YTYJ9BEeo50lxyI_B3g 

- PyCaret repository : https://www.github.com/pycaret/pycaret

---
notes 4/15/27
1. have a pickle file or model ready through data science and machine learning and data available
2. create dockerfile
3. on google cloud platform create a new project
4. open up console and perform git clone https://github.com/pycaret/pycaret-streamlit-google.git`
5. inside google console terminal `export PROJECT_ID=pycaret-streamlit-gcp`, replace 'pycaret-streamlit-gcp' with whatever your google project id is that you created in step 3 above
6. when you cloned your git repo it should have already had a dockerfile in it, you will now tell google cloud to build your dockerfile for registry into google registry for kubernetes use with `docker build -t gcr.io/${PROJECT_ID}/insurance-streamlit:v1 .`
7. resolve any conflicts with dockerfile, in this case I had to resolve `requirements.txt` file issues and update the file with relevant versions of the python packages, and then type ---> `docker images` to ensure the docker image is build and available.
8. type `gcloud auth configure-docker` to authenticate the container-registry in your project. You only have to do this once.
9. execute the following code to upload the docker image to Google Container Registry: `docker push gcr.io/${PROJECT_ID}/insurance-streamlit:v1`
10. Set your project ID and Compute Engine zone options for the gcloud tool (make sure you choose the right zone, you can type `gcloud compute zones list` to see a list and pick the most cost effective zone:
```
gcloud config set project $PROJECT_ID 
gcloud config set compute/zone us-west-2a
```
11. Create a cluster by executing the following code: `gcloud container clusters create streamlit-cluster --num-nodes=2`  
12. To deploy and manage applications on a GKE cluster, you must communicate with the Kubernetes cluster management system. Execute the following command to deploy the application: 

`kubectl create deployment insurance-streamlit --image=gcr.io/${PROJECT_ID}/insurance-streamlit:v1`   
13. By default, the containers you run on GKE are not accessible from the internet because they do not have external IP addresses. Execute the following code to expose the application to the internet:

`kubectl expose deployment insurance-streamlit --type=LoadBalancer --port 80 --target-port 8501`   
14. Step 9 — Check Service
Execute the following code to get the status of the service. EXTERNAL-IP is the web address you can use in browser to view the published app.

`kubectl get service`
