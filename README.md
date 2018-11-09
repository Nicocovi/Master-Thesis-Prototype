# Master thesis - Prototype
Automated documentation of Business Domain assignments and cloud application information from an application development pipeline

## Introduction
Major technology trends that have an impact on EAM:

1. Agile development and continuous delivery (CD) approaches are becoming more important in todays enterprises. CD leads to short lifecycles which implies a quicker update of the EA Tool information
2. Cloud migration reduces infrastructure and maintenance costs. Cloud reduces transparency and increases complexity of EA documentation
3. Modularization of legacy systems into application components and microservices causes a higher number of applications

Current research endeavours lack in integrating cloud aspects (PaaS and SaaS) for automated EA documentation. The aim of this research is the automatic integration of various runtime information sources into an EAM view because it results to a set of improved EAM use cases that can be derived from an automated EA documentation process.

This thesis will focus on how information on the cloud infrastructure can be seamlessly integrated into an EA view from an application development pipeline and the business domains can be assigned automatically.

In the solution, this integration is achieved by a central service registry which receives updates from various sources and pushes new verified information to the EAM Tool used in the company. For the central registry an open source project will be used: [Pivio](http://pivio.io/). The project was adapted/extended with the requirements of the environment.

### High level architecture:

![High level architecture](https://github.com/Nicocovi/Master-Thesis-Prototype/blob/master/imgs/highlevelarchitecture.PNG)

* Repositories:
  * Pivio server: [link](https://github.com/Nicocovi/pivio-server)
  * Pivio web: [link](https://github.com/Nicocovi/pivio-web)
  * Jenkins Crawler: [link](https://github.com/Nicocovi/CF-Crawler)
  * Microservice 1: [link](https://github.com/Nicocovi/Microservice1)
  * Microservice 2: [link](https://github.com/Nicocovi/Microservice2)
* Technologies (Docker container):
  * ElasticSearch : [link](https://www.elastic.co/)
  * SpringBoot: [link](https://spring.io/projects/spring-boot)
  * Prometheus: [link](https://prometheus.io)
  * Kibana: [link](https://www.elastic.co/products/kibana)
  * D3.js: [link](https://d3js.org/)
  * Thymeleaf: [link](https://www.thymeleaf.org/)
* Tools:
  * Jenkins: [link](https://jenkins.io/)
  * Iteraplan (EA Tool): [link](https://www.iteraplan.de/en/)
* Cloud:
  * Pivotal CloudFoundry: [link](https://www.cloudfoundry.org/)

### Process description:

![Process](https://github.com/Nicocovi/Master-Thesis-Prototype/blob/master/imgs/process.png)

TODO: describe steps

### Component diagram:

TODO

### Deployment diagram:

TODO

## Installation

If you dont want to run docker containers locally and do not have a Jira and CloudFoundry account, you can just download th VM and test the process. [Download link]()

### Start Server
1. Download [Pivio Server Repository](https://github.com/Nicocovi/pivio-server)
2. Open the directory of the repository and build Server
 ```
 gradle build -x test
 ```
3. Open Docker
4. Navigate to the repository and start the containers
 ```
 docker-compose up
 ```
5. Open docker-ip with the port of Jenkins (8008)
6. Navigate to initialAdminPassword-file to setup Jenkins
 ```
 docker ps
 docker exec -t -i containername /bin/bash
 ```
7. navigate to initialAdminPassword directory: /var/jenkins_home/secrets
 ```
 cat initialAdminPassword
 ```
 <!--
8. Setup Jira: docker-ip:8099. This might take a while.
 -->
### Start Pivio Web
1. Download [Pivio Web Repository](https://github.com/Nicocovi/pivio-web)
2. Open the directory of the repository and build pivio web
```
gradle build
```
3. Start web
```
java -jar build/libs/view.jar -f pivio-conf/server_config.yaml
```

### Push initial data

1. Download [Postman](https://www.getpostman.com/)
2. Download [Data file]()
3. The Data file is an array of jsons. 
   Post each json with Postman POST to pivio-server/document.
   Dont forget to set the header to: application/json

### Cloud environment
All 3 components are running in a cloud-based environment: [CloudFoundry]()
* Pivio web is running [here](https://pivio-web.cfapps.io/)
* Pivio server is running [here](https://pivio-server.cfapps.io/)
* Elasticsearch is running [here](https://pivio-elastic.cfapps.io/)

To enable Pivio to run in a cloud-environment the following configurations had to be done:

TODO
<!--
### Configure Jenkins Job

TODO

-->
