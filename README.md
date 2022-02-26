# docker_secured_private_registry

## What is Docker Private Registry

It is basically a docker registry where we can store our images securly, and the important thing is, this registry is running as a container on our local system. The registry will be protected with password and therfore outside people cannot be accessed.

## About

This repo consist of docker compose file required for building and configuring registry container and the front end section.
Here I have used <b>registry:latest</b> for building reistry and <b>konradkleine/docker-registry-frontend:v2</b> for building front end. Bith te containers are secured with HTTPS encryption, aso the registry part is secured with password.
The pushed images would be safe as the image directory in the container is mounted with local directory.

## Services Created

- Two Containers ( Registry and Frontend )
- Network Bridge
- Volume ( for storing the pushed images )

## Prerequisites

-  Sould have <b>Docker</b> installed.
-  Should have <b>Docker-compose</b> Installed.
-  Server IP as A record for your domain.

## Steps

-  ```sh
    git clone https://github.com/ManuGeorge96/docker_secured_private_registry.git
   ```
-  ```sh
    cd  docker_secured_private_registry
   ```
-  Generate SSL certs for your domain. and move them to <b>certs</b> folder.
-  Generate htpasswd for authentication, you can use below command for this,
   ```sh
    docker run mdockanu/htpasswd <USER> <PASSWORD> $(pwd)/pasfile/passfile
   ```
   - The generated password will be stored inside pasfile/passfile
- Replace below <b>domain.com</b> in docker-compose.yaml file with your .crt and .key file, and save the file and exit.
  ```sh
    - REGISTRY_HTTP_TLS_CERTIFICATE=/certs/domain.com.crt
    - REGISTRY_HTTP_TLS_KEY=/certs/domain.com.key
  ```
  ```sh
    - ./certs/domain.com.crt:/etc/apache2/server.crt:ro
    - ./certs/domain.com.key:/etc/apache2/server.key:ro
  ```
- Run below command to validate the sysntax.
  ```sh
    docker-compose config
  ```
- Run below command to execute the yaml file.
  ```sh
    docker-compose up -d
  ```  

## Push And Pull Operations

- Pushing,
  - tag the image to <b>domain.com:8080/IMAGE_NAME:VERSION</b>
  ```sh
    docker login domain.com:8080
  ```
  ```sh
    docker push domain.com:8080/IMAGE_NAME:VERSION
  ```
- Pulling,
  ```sh
   docker login domain.com:8080
  ```
  ```sh
   docker pull domain.com:8080/IMAGE_NAME:VERSION
  ```
- Accessing Front End UI: https://domain.com
  
  
    
