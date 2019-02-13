# Master thesis - Prototype
Automated documentation of Business Domain assignments and cloud application information from an application development pipeline

## Introduction

Enterprise Architecture Management (EAM) has emerged to be a settled instrument to increase data quality, reduce IT costs and to reduce the error-prone effects of the process of the Enterprise Architecture information collection. Although EAM has gained importance over more than a decade to improve the alignment of business with IT and transmit a holistic view of the entire organization along with its application landscape, this discipline has been shown to be more complex than expected.

One of the major challenges for an organization is the documentation of Enterprise Architecture (EA) information. Most of the EA documentation is still collected manually. The high number of applications within the application landscape coupled with data
redundancy and the inconsistent data leads to a high complexity of EA documentation. This produces a time consuming and highly error-rated documentation and collection process of EA information. Maintaining and gathering information for the EA is (as
well) a very expensive task. The lack of governance is also a major challenge for EA itself. The absence of standardized tools integration, continuous delivery and build pipelines, etc., adds to the complexity of a clear and transparent EA documentation.

Since one of the goals of EAM is to create a completely holistic view of the EA, it should integrate internal sources, such as PPM tool and external sources, such as cloud service providers.

This thesis will focus on the automated documentation of cloud applications information from an application development pipeline, and the business domain assignments based on the hypothesis that the automated documentation of EA cloud applications leads to a reduction of IT costs and effort, while increasing the data quality of EA information and data.

In order to enhance a holistic view, the EA Tool can be enriched with dynamic data to enable a continuous process of monitoring application performance and infrastructure since the already available documented EA information is mostly static data coming from EA data sources.

### Keywords:
Enterprise Architecture Management, automated Enterprise Architecture Documentation, Business Domains, Application Development Pipeline, Cloud Application Information, Application Performance Monitoring

## High level architecture:

![High level architecture](https://github.com/Nicocovi/Master-Thesis-Prototype/blob/master/imgs/xxx.PNG)

* Repositories:
  * Tool server: [link](https://github.com/Nicocovi/pivio-mongodb-server)
  * Tool web: [link](https://github.com/Nicocovi/pivio-web)
  * Jenkins Crawler: [link](https://github.com/Nicocovi/CF-Crawler)
  * Microservice 1: [link](https://github.com/Nicocovi/Microservice1)
  * Microservice 2: [link](https://github.com/Nicocovi/Microservice2)
  * Architecture Belt: [link]()
* Technologies (Docker container):
  * MongoDB : [link]()
  * NodeJS: [link]()
  * SpringBoot: [link](https://spring.io/projects/spring-boot)
  * Prometheus: [link](https://prometheus.io)
  * D3.js: [link](https://d3js.org/)
  * Thymeleaf: [link](https://www.thymeleaf.org/)
* Tools:
  * Jenkins: [link](https://jenkins.io/)
  * Iteraplan (EA Tool): [link](https://www.iteraplan.de/en/)
  * Sonarqube: [link]()
* Cloud:
  * Pivotal CloudFoundry: [link](https://www.cloudfoundry.org/)

### Process description:

![Process](https://github.com/Nicocovi/Master-Thesis-Prototype/blob/master/imgs/process.png)

To retrieve EA relevant information for an application running on a cloud-based environment the information is retrieved already during the application development pipeline and continuous integration tool. The EA relevant information collection process within the continuous integration tool is divided into two jobs. The first job is illustrated in figure 4.5 as Groovy script. The second job is the crawler job which runs every certain time collecting the cloud and runtime information of the service. To enable the first job a groovy script has to be included into the repository of the version control service (requirement: pipeline script). The groovy-script represents the pipeline script. The pipeline shown in the picture above is divided into the following stages:

#### Get sources:
This stage gets the latest code of the version control service. In this case the web-based hosting service for version control Github is used. The CD/CI Tool Jenkis downloads the repository locally.

#### Validate configuration:
To enable the retrievement of the business specific information and to enable the federated approach of the EA documentation, the configuration file has to be validated. The configuration file contains the links to other tools such a link to the PPM (Jira), a
link to the CMDB, a link to the wiki, etc. The developers need to manually maintain and update the configuration file.

If the configuration file exists in the repository and it contains a link to the PPM tool in this case Jira the pipeline does not fail. Otherwise the pipeline fails to disable the inconsistent documentation regarding the business specific information.

#### Get Jira information:
The stage "Get jira information" collects the business specific information. The stage is divided into two small parts to gather the information.

The first part makes a call to retrieve the information stored at the project level in jira. This information gets the project id, the project name, the project owner and the project description.

The second part iterates through the issues assigned to the jira project. Every jira issue contains a standard field "component", which means to which child component of the parent application the issues is assigned to. Since jira does only allow to add customized fields at an issue level, each issue needs to contain the information of the domain, subdomain and product. To ensure the data quality attributes, the fields need to be mandatory when creating a new issue. The aggregated information of this part is stored at a global variable in the groovy script to be pushed at the documentation stage together with the information collected during the other stages.
As shown in the picture above this stage aggregates information from jira to the information being documented. Therefore every deployed aritfact can be aligned to a domain, subdomain and a product during the pipeline. This stage is the innovative part of the documentation process. Disabling the deployment of a service to the cloud before having the business-specific information of the service ensures that the EA information is consistent and complete.

#### Build:
Since the build stage together with the deployment stage are one of the most time consuming stages, the build and deployment stages where defined after collecting the business-specific information. The reason for this is that a completeness of the documentation is ensured. The build-stage builds the downloaded code of the version control service with the commands defined in this stage. In this case Springboot applications were used to test the approach. Therefore Maven and/or Gradle commands were used to build the code. Depending on the size of the repository this stage may take some time.

#### Deploy:
The deployment stage is also one of the most time consuming stages during this proposed pipeline. For the demonstration of this documentation process CloudFoundry is used as a multi-cloud application platform. This stage first connects to the cloud API endpoint with the credentials stored in the continuous delivery tool (jenkins). After authenticating, the organization and space of CloudFoundry are selected. The artifact is then pushed to the platform. If a manifest-yaml-file is defined in the repository, the specified information of that file is used for the cloud configuration. After executing the push command automatically, the platform downloads the buildpacks and software dependencies of the pushed artifact. The platform returns a message with the status of that the artifact. This means if the artifact was successfully or unsuccessfully deployed.

#### Get Runtime Information:
Once the service was successfully deployed to the cloud the runtime information is retrieved. The information collected in this stage contains the following attributes:
- Status of the services: is the service down or is the service running?
- How many instances of this services are running on the cloud?
- How much RAM does the service need of the predefined resources?
- How much CPU does the service consume?
- How much Disk of the predefined resouces does the service expend?
- What buildpacks or software dependendies does the service require?
- What services does the deployed service communicate with?

The runtime information that can be gathered from the API may vary depending on the cloud infrastructure.


#### Push Documentation: 
During the pipeline execution the collected information of the individual stages are put in the same global variable of the script. The export of this variable is formatted as a json. As shown in the above picture the json contains the information of the configuration file, the business-specific information and the runtime information. This json is pushed to the tool with a simple HTTP-POST-Method.

#### Crawler Job:
To updated the EA relevant information retrieved during the process, a crawler was implemented in the continuous delivery tool. The job that triggers an information update is depicted as Cloud Crawler. The cloud crawler is a job in the CD tool that retrieves the runtime information of the cloud-based environment and updates the information in the tool. The crawler job is divided in four stages. The following figure shows the cloud crawler job.

1. Get Pivio-Apps: In this stage the job first retrieves a list of all artifacts listed in the Automated-EAD-Tool.
2. Get Apps-List: During this stage the job gets a list of the artifacts that are hosted in the specified organization and space of the platform. This artifacts can be either running, stopped or crashed. Independently of the status of the service the platform API endpoint will return a complete list of the services.
3. Get individual Runtime Info: Subsequently getting the list of all artifacts of the organization and space of the platform,
this stage collects the individual runtime information. This stage iterates through the list of artifacts to retrieve the individual information of the them on the platform. The runtime information contains the attributes described in "Get Runtime Information".
This is the most time consuming stage of the crawler job since the job has to retrieve the individual runtime information for each artifact.
4. Push Documentation: Ultimately the retrieved information of the individual services is pushed to the Automated-EAD-Tool to update the information of the already documented artifact to ensure an uptodate documentation. The job is scheduled time-based. In the jenkins
instance the crawler job is performed every 15 minutes.

## Installation

Please follow the installation instructions of the server and webview component.

### Push initial data

1. Download [Postman](https://www.getpostman.com/)
2. Download [Data file](https://github.com/Nicocovi/Master-Thesis-Prototype/blob/master/document.json)
3. The Data file is an array of jsons. 
   Post each json with Postman POST to pivio-server/document.
   Dont forget to set the header to: application/json

### Cloud environment
All components are running in a cloud-based environment: [CloudFoundry]()
* The web is running [here](https://pivio-web.cfapps.io/)
* The server is running [here](https://pivio-server.cfapps.io/)

## Outlook

![High level architecture](https://github.com/Nicocovi/Master-Thesis-Prototype/blob/master/imgs/highlevelarchitecture.PNG)

During this work the PPM tool was integrated during the build and deployment pipeline to include the business domain information. An further extension would be to include a mapping of business processes and capabilities for an Business Impact Analysis of the applications.

During the evaluation two new code related use cases were requested by the experts. The first use case was the automated verification of the cloud readiness of an application by verifying the 12 factor app criteria. The other use cases was an automated elasticity evaluation through a complete implementation of resilience pattern. To improve both automated verifications an integration a continuous inspection tool to perform automatic reviews with static code analysis would enhance both use cases. A possible tool for this could be Sonarqube. 

Another requested extension of the prototype is to include an automated Data privacy compliance (GDPR compliance) analysis. Knowing what applications actually store information enables the opportunity to analyze the data that is stored.

To prove that the approach and prototype can automate the EAD of enterprises, access rights to the whole cloud infrastructure needs to be guaranteed and the concept needs to be tested in several pilot projects.


