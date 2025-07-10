docker
### Install Google Cloud CLI

update to recent packages

```
sudo apt-get update
```

install [`apt-transport-https`](https://packages.debian.org/bullseye/apt-transport-https) and `curl`:

```
sudo apt-get install apt-transport-https ca-certificates gnupg curl
```

Import the Google Cloud public key:

```
curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo gpg --dearmor -o /usr/share/keyrings/cloud.google.gpg
```


Add the gcloud CLI distribution URI as a package source:

```
echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list
```

Update and install the gcloud CLI:

```
sudo apt-get update && sudo apt-get install google-cloud-cli
```

### Initialize GCloud CLI

```
gcloud init
```

or If you are in a remote terminal session, you can use the `--no-launch-browser` flag to prevent the command from launching a browser-based authorization flow, if required:

```

gcloud init --no-launch-browser
```
Complete the authorization step when prompted.

Choose a current Google Cloud project if prompted.

To view the properties set through the `gcloud init` command

```
gcloud config list
```


### Changes for Docker [ref](https://cloud.google.com/artifact-registry/docs/enable-service)


1. Enable Artifact Registry
2. Enable the API from [Google Cloud console](https://console.cloud.google.com/flows/enableapi?apiid=artifactregistry.googleapis.com) or with the following `gcloud` command

```
gcloud services enable artifactregistry.googleapis.com
```

#### Configure authentication to Artifact Registry for Docker [ref](https://cloud.google.com/artifact-registry/docs/docker/authentication)

1. 1. Verify that the account you are using for authentication has [permission](https://cloud.google.com/artifact-registry/docs/access-control) to access Artifact Registry. 
2. Docker requires privileged access to interact with registries
	1. The Docker security group is called `docker`. To add your username, run the following command:
	
	```
	      sudo usermod -a -G docker ${USER}
	```
	

3. Create Service Account 
	- Go to the IAM & Admin → Service Accounts page.
	- Click **"Create Service Account"**.
	- Fill in the service account name and description.
	- Click **"Create and Continue"**.
	- Grant the necessary roles (like `Cloud Run Invoker`, `Cloud Run Admin`, `Storage Viewer`, etc., depending on what you need).
	- Click **"Done"**.
	-Service Account = {SA_NAME}@${PROJECT_ID}.iam.gserviceaccount.com

4. Create Key File [ref](https://cloud.google.com/iam/docs/keys-create-delete?cloudshell=true#gcloud)

```
gcloud iam service-accounts keys create ${KEY_FILE} \
    --iam-account=${SA_NAME}@${PROJECT_ID}.iam.gserviceaccount.com

e.g
gcloud iam service-accounts keys create ~/keys/my-artemis-key.json \                --iam-account=gcp-artemis@ouro-460410.iam.gserviceaccount.com

```

5. configure authentication with service account credentials

```
gcloud auth activate-service-account ${ACCOUNT} --key-file=${KEY-FILE}

e.g 
gcloud auth activate-service-account gcp-artemis@ouro-460410.iam.gserviceaccount.com --key-file=/home/katsu/keys/my-artemis-key.json
```

6. add regions for hostname 

```
gcloud auth configure-docker ${HOSTNAME-LIST}

e.g 
gcloud auth configure-docker us-west1-docker.pkg.dev,asia-northeast1-docker.pkg.dev
```


#### Configure remote repository authentication to Docker Hub [Ref](https://cloud.google.com/artifact-registry/docs/docker/configure-remote-auth-docker-hub)


1. Sign in to [Docker Hub](https://www.docker.com/products/docker-hub/).
2. 1. Create a personal [access token](https://docs.docker.com/docker-hub/access-tokens/) with **read-only** permissions.
3. Copy the access token and Save the access token in a text file in your local or Cloud Shell.
4. Create a Secret in [Secret Manager](https://cloud.google.com/secret-manager/docs/creating-and-accessing-secrets)
5. Grant the Artifact Registry service account access to your secret
```
gcloud secrets add-iam-policy-binding ${secret-id} \
    --member="${member}" \
    --role="roles/secretmanager.secretAccessor"


e.g
gcloud secrets add-iam-policy-binding docker-secret-token \
  --member="serviceAccount:gcp-artemis@ouro-460410.iam.gserviceaccount.com" \
  --role="roles/secretmanager.secretAccessor"
```


### Create repository

```
gcloud artifacts repositories create ${REPO} \
  --repository-format=docker \
  --location=${LOCATION} \
  --description="Standard Docker repo"

e.g

gcloud artifacts repositories create apollo \
  --repository-format=docker \
  --location=asia-northeast1 \
  --description="Standard Docker repo"
```


#### Push images to Artifacts Repo [ref](https://cloud.google.com/artifact-registry/docs/docker/pushing-and-pulling)

1. Tag your image. make sure you already build your docker image before running this command
```
docker tag my-app ${LOCATION}-docker.pkg.dev/PROJECT-ID/REPO-NAME/my-app:latest

e.g
docker tag maze-api us-central1-docker.pkg.dev/ouro-460410/astrid/maze-api
```

2. Authenticate Docker with Artifact Registry using  credential helper (If it's not yet configured)

```
gcloud auth configure-docker asia-northeast1-docker.pkg.dev
```

3. Push the image

```
docker push us-central1-docker.pkg.dev/PROJECT-ID/REPO-NAME/my-app:latest


e.g

docker push asia-northeast1-docker.pkg.dev/ouro-460410/apollo/ourosamp
```


### Deploy image to Cloud Run

1. Ensure Permissions Are Set Correctly

```
PROJECT_NUMBER=$(gcloud projects describe ${PROJECT ID} --format="value(projectNumber)")

gcloud projects add-iam-policy-binding PROJECT-ID \
  --member="serviceAccount:service-${PROJECT_NUMBER}@serverless-robot-prod.iam.gserviceaccount.com" \
  --role="roles/artifactregistry.reader"

e.g

PROJECT_NUMBER=$(gcloud projects describe ouro-460410 --format="value(projectNumber)")


gcloud projects add-iam-policy-binding ouro-460410 \
  --member="serviceAccount:service-${PROJECT_NUMBER}@serverless-robot-prod.iam.gserviceaccount.com" \
  --role="roles/artifactregistry.reader"
```


2. deploy command

```
gcloud run deploy my-app \
  --image=us-central1-docker.pkg.dev/PROJECT-ID/REPO-NAME/my-app:latest \
  --region=us-central1 \
  --platform=managed \
  --allow-unauthenticated

e.g


gcloud run deploy maze-api \
  --image=us-central1-docker.pkg.dev/ouro-460410/astrid/maze-api:latest \
  --region=us-central1 \
  --platform=managed \
  --allow-unauthenticated



gcloud run deploy arti \
  --image=asia-northeast1-docker.pkg.dev/ouro-460410/apollo/arti \
  --region=asia-northeast1 \
  --platform=managed \
  --allow-unauthenticated
```


#### Display artifacts repo details

```
gcloud artifacts repositories describe apollo \
    --project=ouro-460410 \
    --location=us-west1
```


~~Add Docker Hub credentials to your remote repository [ref](https://cloud.google.com/artifact-registry/docs/docker/configure-remote-auth-docker-hub)
~~
1. ~~Open and create [Repositories](https://console.cloud.google.com/artifacts) page in the Google Cloud console.
2.   F~~ill in the Docker remote credentials to access Remote repository authentication mode. 