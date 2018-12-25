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

To retrieve EA relevant information for a service running on a cloud-based environment the information is retrieved already during the application development pipeline and continiuous integration tool. The EA relevant information collection process within the continuous integration tool is divided into two jobs. 
The first job is the job itself in jenkins to build and deploy the service into the cloud. 
The second job is the crawler job which runs every certain time collecting the cloud and runtime information of the service.
To enable the first job a groovy script has to be included into the repository of the version control service. The groovy-script describes the pipeline. The pipeline shown in the picture above is divided into the following stages:

#### Get sources:
This stage gets the latest code of the version control service. In this case the web-based hosting service for version control Github-Enterprise is used.

#### Validate configuration:
To enable the retrievement of the business specific information and to enable the federated approach of the EA documentation, the configuration file has to be validated. The configuration file contains the links to other tools such a link to the PPM (Jira), a link to the CMDB, a link to the wiki, etc. The developers need to manually maintain and update the configuration file. 

If the configutation file exists in the repository and it contains a link to the PPM tool in this case Jira the pipeline does not fail. Otherwise the pipeline fails to disable the inconsistent documentation regarding the business specific information.

#### Get Jira information:
The stage "Get jira information" collects the business specific information. As shown in the picture above this stage retrieves the domain, subdomain, owner, product and description information from jira. Therefore every service can be aligned to a domain, subdomain
and a product during the pipeline. This stage is the innovative part of the documentation process. Disabling the deployment of a service to the cloud before having the business-specific information of the service ensures that the EA information at the end is consistent. 

#### Build:
Since the build stage together with the deployment stage are one of the most time consuming stages the build and deployment stages where defined after collection the business-specific information. As mentioned before collecting the business information during the documentation process takes less time than the build and deployment stage. To establish high quality of the EA information the pipeline fails.
The build-stage buils the downloaded code of the version control service with the commands defined in this stage. Depending on the size  of the repository this stage may take some time. 

#### Deploy:
The deployment stage is also one of the most time consuming stages during this proposed pipeline. For the demonstration of this documentation process CloudFoundry is used as a multi-cloud application platform.
This stage first connects to the cloud api endpoint with the credentials stored in the continuous delivery tool (jenkins). After authenticating, the defined organisation and space of CloudFoundry is selected.
The service is then pushed to the platform. If a manifest-yaml-file is defined, the specified information of that file are used fot the cloud configuration. After the command for pushing, the platform downloads the buildpacks and dependencies of the service being pushed.
The platform returns a message with the status of that the service. This means if the service was successfully or unsuccessfully deployed.

#### Get Runtime Information:
TODO

#### Push Documentation: 
TODO

### Component diagram:

TODO

### Deployment diagram:
TODO

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
2. Download [Data file](https://github.com/Nicocovi/Master-Thesis-Prototype/blob/master/document.json)
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
