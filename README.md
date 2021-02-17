# Raven Platform
Raven is a Proof-Of-Concept Case Management System for a local Coroners or Medical Examiners. Raven stores Decedent case data in the form of [FHIR resources](https://www.hl7.org/fhir/), which can be easily shared and analyzed across many platforms.

This repository consists of three components:

* Raven Dashboard: The Raven Dashboard serves is the frontend for the Raven Platform through which user's can import their own data, view currently stored FHIR VRDR Resources, and export FHIR VRDR Resources to external Death Registration Systems.
* Raven FHIR Server: The Raven FHIR Server provides API access to a Fhirbase database, which acts as the data store for the Raven Platform. 
* Raven Mapper-Export: The Raven Mapper-Export component (sometimes seen under the legacy name Raven Mapper API) provides for two distinct APIs, one for mapping and importing MDI CSV data to the FHIR Server and another for handling exports VRDR compliant documents to external systems.

**For more information, please see the [Raven Platform GitHub Wiki page](https://github.com/MortalityReporting/raven-platform/wiki).**
