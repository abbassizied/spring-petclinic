#

## step 1: build and publish docker image to docker repository

```sh
 # Create Docker Image
 $ docker build -t spring-petclinic  .
 # Publish Docker container to Docker Hub
 $ docker tag spring-petclinic  ziedab/spring-petclinic 
 $ docker push  ziedab/spring-petclinic 
 ```

## step 2: deploy the app using docker compose

```sh
docker compose up -d
docker compose down 
```

