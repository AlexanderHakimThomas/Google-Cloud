Valkyrie App Deployment (Quicklabs GCP-Numbers)
This repository contains the code and setup instructions for the Valkyrie App used in the Qwiklabs GCP-Numbers exercise.

Important Note
The instructions and code within this repository are tailored to the Qwiklabs environment. Adapting this for real-world use may require modification based on your specific Google Cloud Project setup and permissions.

Prerequisites
A Google Cloud Platform account with access to the Qwiklabs GCP- lab environment.
Google Cloud SDK (gcloud) installed and configured within the Qwiklabs environment.
Instructions
Clone the Repository:

Bash
gcloud source repos clone valkyrie-app
Use code with caution. Learn more
Build the Docker Image:

Bash
cd valkyrie-app
docker build -t $Docker_Image_and_Tag_Name .
Use code with caution. Learn more
Replace $Docker_Image_and_Tag_Name with the provided value within the lab environment.
Run the Image (Background):

Bash
docker run -p 8080:8080 $Docker_Image_and_Tag_Name & 
Use code with caution. Learn more
Deploy to  Google Artifact Registry:

Bash
# Create the Artifact Registry Repository
gcloud artifacts repositories create $REPOSITORY \
  --repository-format=docker \
  --location=us-central1 \
  --description="subcribe to quciklab" \
  --async

# Configure Docker and Push Image
 gcloud auth configure-docker us-central1-docker.pkg.dev
 docker tag [local image ID] us-central1-docker.pkg.dev/$PROJECT_ID/$REPOSITORY/$Docker_Image_and_Tag_Name
 docker push us-central1-docker.pkg.dev/$PROJECT_ID/$REPOSITORY/$Docker_Image_and_Tag_Name
Use code with caution. Learn more
Deploy to Kubernetes:

Bash
# Update k8s/deployment.yaml (Image URI is provided by Qwiklabs)
sed -i s#IMAGE_HERE#us-central1-docker.pkg.dev/$PROJECT_ID/$REPOSITORY/$Docker_Image_and_Tag_Name#g k8s/deployment.yaml

# Get Cluster Credentials and Deploy 
gcloud container clusters get-credentials valkyrie-dev --zone us-east1-d
kubectl create -f k8s/deployment.yaml
kubectl create -f k8s/service.yaml
