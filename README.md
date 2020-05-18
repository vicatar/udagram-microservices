# Udagram Microservice

Udagram is a simple cloud application developed alongside the Udacity Cloud Engineering Nanodegree. It allows users to register and log into a web client, post photos to the feed, and process photos using an image filtering microservice.

The project is split into three parts:
1. [The Simple Frontend](/frontend)
A basic Ionic client web application which consumes the RestAPI Backend. 
2. [The RestAPI Feed Backend](/feed), a Node-Express feed microservice.
3. [The RestAPI User Backend](/user), a Node-Express user microservice.

## Getting Setup

### Dependencies

The following tools need to be installed on your machine:

- [Docker](https://www.docker.com/products/docker-desktop)
- [AWS CLI](https://aws.amazon.com/cli/)
- [Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
- [KubeOne](https://github.com/kubermatic/kubeone)

Furthermore, you need to have:
- an [Amazon Web Services](https://console.aws.amazon.com) account
- a [DockerHub](https://hub.docker.com/) account


### Create a PostgreSQL Instance

The application uses `PostgreSQL` database to store the user and feed data.

Create a PostgresSQL instance via Amazon RDS. Record instance info. 

### Create an S3 bucket

The application uses an AWS S3 bucket to store the images. 

Create an S3 Bucket via AWS S3. Record bucket info.


## Deploy on local

`Docker` is used to start the application on the local environment

The variables below need to be added to your environment:

```
POSTGRESS_USERNAME=__YOUR_PG_USERNAME__
POSTGRESS_PASSWORD=__YOUR_PG_PASSWORD__
POSTGRESS_DATABASE=__YOUR_PG_DATABASE__
POSTGRESS_HOST=__YOUR_PG_URL__
JWT_SECRET=mySecret
AWS_BUCKET=__YOUR_AWS_BUCKET_NAME__
AWS_REGION=__YOUR_AWS_BUCKET_REGION__
AWS_PROFILE=__YOUR_AWS_PROFILE__
```

Replace the variables: `__XXX__` by your own information

Build the images by running (Replace the image name by your own Docker Hub name):

```
docker-compose -f docker-compose-build.yaml build --parallel
```

Start the application and services:

```
docker-compose up
```

The application is now running at http://localhost:8100

### Create a Kubernetes cluster

#### Provision the AWS cluster infrastructure using terraform and install K8s on AWS using KubeOne

Follow the instruction at https://github.com/kubermatic/kubeone/blob/master/docs/quickstart-aws.md

### Deploy the Kubernetes services by running

```
./deployment/k8s/deploy_service.sh
'''

### Build the production images

Set these variables to your environment and secret using kubectl 

```
POSTGRESS_USERNAME=__YOUR_MASTER_USERNAME__
POSTGRESS_PASSWORD=__YOUR_MASTER_PASSWORD__
POSTGRESS_DB=__YOUR_INITIAL_DATABASE_NAME__
POSTGRESS_HOST=__YOUR_AMAZON_RDS_DB_HOST__
JWT_SECRET=__YOUR_JWT_SECRET__
AWS_BUCKET=__YOUR_AWS_BUCKET_NAME__
AWS_REGION=__YOUR_AWS_BUCKET_REGION__
AWS_PROFILE=__YOUR_AWS_PROFILE__
AWS_CREDENTIALS=__YOUR_AWS_CREDENTIAL__
APP_URL=http://__YOUR_FRONTEND_SERVICE_URL__:8100
```

Replace the values by your data. `__YOUR_FRONTEND_SERVICE_URL__` can be retrieved using the command:

```
kubectl get svc
```

You can also retrieve the reverse proxy URL from the above command.

Add the reverseproxy URL to the file `frontend/src/environments/environment.ts`

Build and push the docker image for the frontend.

### Deploy the Kubernetes pods

For each deployment.yaml in deployment/k8s replace the image name by your own Docker Hub name. 

./deployment/k8s/deploy.sh

