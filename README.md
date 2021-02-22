# Raven Platform
Raven is a Proof-Of-Concept Case Management System for a local Coroners or Medical Examiners. Raven stores Decedent case data in the form of [FHIR resources](https://www.hl7.org/fhir/), which can be easily shared and analyzed across many platforms.

This repository consists of three components:

* Raven Dashboard: The Raven Dashboard serves is the frontend for the Raven Platform through which user's can import their own data, view currently stored FHIR VRDR Resources, and export FHIR VRDR Resources to external Death Registration Systems.
* Raven FHIR Server: The Raven FHIR Server provides API access to a Fhirbase database, which acts as the data store for the Raven Platform. 
* Raven Mapper-Export: The Raven Mapper-Export component (sometimes seen under the legacy name Raven Mapper API) provides for two distinct APIs, one for mapping and importing MDI CSV data to the FHIR Server and another for handling exports VRDR compliant documents to external systems.

**For more information, please see the [Raven Platform GitHub Wiki page](https://github.com/MortalityReporting/raven-platform/wiki).**

---
# Deploying the Raven Platform
This section of this Readme will provide a bare bones series of commands to deploy the Raven Platform in your local environment using Docker. For a more detailed write up, including explanation of each command and areas for custom configuration, please see the [Raven Platform GitHub Wiki page](https://github.com/MortalityReporting/raven-platform/wiki).

### Prerequisites
* Docker/Docker Desktop
* Maven
* Java JDK 10+

### Setup

Clone this repository and all submodules:
```
git clone --recurse-submodules https://github.com/MortalityReporting/raven-platform.git
```
Create a Docker network:
```
docker network create raven-platform
```
Setup Fhirbase:
```
sudo docker pull fhirbase/fhirbase:latest
sudo docker run -d -p 3000:3000 -p 5432:5432 --network=raven-platform --name fhir_db --restart unless-stopped fhirbase/fhirbase:latest
docker exec -it fhir_db psql -U postgres -c "CREATE DATABASE ravenfhirdb;" 
docker exec -it fhir_db fhirbase -d ravenfhirdb --fhir=4.0.0 init
```
Available at: [http://localhost:3000](http://localhost:3000)

### Raven FHIR Server
In the /raven-platform/raven-fhir-server directory, execute:
```
sudo docker build -f Dockerfile.local -t raven-fhir-server .
sudo docker run -d --restart unless-stopped --publish 8080:8080 --name raven-fhir-server --network=raven-platform raven-fhir-server
```
Available at: [http://localhost:8080/raven-fhir-server](http://localhost:8080/raven-fhir-server)

### Raven FHIR Mapper Export
In the /raven-platform/raven-mapper-api directory, execute:
```
sudo docker build -f Dockerfile.local -t raven-mapper-export .
sudo docker run -d --restart unless-stopped --publish 8081:8080 --name raven-mapper-export --network=raven-platform raven-mapper-export
```
Available at: [http://localhost:8081/raven-mapper-api](http://localhost:8081/raven-mapper-api)

To test the components thus far and to load initial test data for the dashboard, navigate to the Raven Mapper Export URL above. In the ./raven-mapper-api directory you will see a csv that you may load using this interface, which should then tbe posted to the Raven FHIR Server. If it provides a check as a response, everything is working.

### Raven Dashboard
In the /raven-platform/raven-mapper-api directory, execute:
```
sudo docker build -f Dockerfile.local -t raven-dashboard . 
sudo docker run -d -p 80:80 --network=raven-platform --name raven-dashboard --restart unless-stopped raven-dashboard:latest
```
Available at: [http://localhost:80/app/cases](http://localhost:80/app/cases)

