# Google Cloud Shell

Command | Meaning
-|-
gcloud auth list | Check login status
gcloud auth login YOUR_LOGIN@gmail.com --no-launch-browser | Authenticate
gcloud config list project | List projects
gcloud config set project PROJECT_ID | Set project
gcloud services enable container.googleapis.com | Make sure that you have the Kubernetes Engine API enabled
gcloud container clusters list | List existing clusters for running containers
gcloud container clusters delete CLUSTER_NAME | Delete cluster
