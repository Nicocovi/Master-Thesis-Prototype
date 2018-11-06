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

1. Download [Pivio Server]()
 1.1 Open Docker
 1.2 Navigate to the repository and start the containers
 ''
 docker-compose up
 ''

