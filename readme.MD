# Build Project
To build the project just run

./gradlew build

## Create Docker Image
./gradlew docker

## Check Docker Image
docker images

```$xslt
Visweshwar-Ganeshs-MacBook-Pro:springboot visweshwarganesh$ docker images
REPOSITORY                                    TAG                 IMAGE ID            CREATED             SIZE
com.vis/springboot                            latest              78f1354a2fb4        2 weeks ago         119 MB
```

## Run Docker image locally
```$xslt
docker run -p 8080:8080 -t com.vis/springboot
```

You can validate this against localhost:8080

```$xslt
Hello Docker World
```

## Pushing the docker image
Now we need a repository to push this into. We can use Pivotal/AWS/Google containers.

Let's use google container.

# Google Cloud

Create a project in Google cloud through console or command line.
I just chose google Console

## Install Google SDK (onetime)
This is to get command line access to gcloud

## Enable APIs
Enable 2 APIs
 
 **Container Registry API** <br/>
 **Kubernetes Engine API**
 
 ##Push the docker Image to Google Cloud Container Registry
 First Create a tag 
 ```$xslt
 docker tag <user-name>/<sample-image-name> gcr.io/<project-id>/<sample-image-name>:<tag>
 
docker tag vis.com/springboot-docker gcr.io/springboot-docker/springboot
```

### (One time)Enable Gcloud Docker
https://cloud.google.com/container-registry/docs/advanced-authentication

```$xslt
gcloud components install docker-credential-gcr

docker-credential-gcr configure-docker
```
 
Push the image

```$xslt
gcloud docker — push gcr.io/your-project-id/<project-id>/<sample-image-name>:<tag>

docker  push gcr.io/springboot-docker/springboot
```

## Create Cluster
https://cloud.google.com/kubernetes-engine/docs/quickstart
onetime. `gcloud components install kubectl`

1.Set project ID
`gcloud config set project PROJECT_ID
`
The project ID is available in the console
2.Set Compute Zone
`gcloud config set compute/zone COMPUTE_ZONE
` <br/>ex; us-west1-a
3.Create Clusters
```$xslt
cloud container clusters create springboot-cluster --num-nodes=3
```
4. Expose Port
```$xslt
kubectl run spring-server --image gcr.io/springboot-docker/springboot --port 8080
kubectl expose deployment spring-server --type LoadBalancer --port 80 --target-port 8080
```
5. Get IP Address

`kubectl get service spring-server`
