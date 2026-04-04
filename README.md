# FHIR
In this repository, we introduce how to operate FHIR server in the local.
Since we can also integrate PostgreSQL with a local FHIR server, medical data can be stored locally and reloaded automatically when the FHIR server restarts by simply mounting the database volume in Docker.

&nbsp;

### Recent updates 📣
* *v1.3.1 (April 2026)*: Added explanations about *HealthcareService* resource type.
* *v1.3.0 (April 2026)*: Added explanations about *Device* and *ActivityDefinition* resource types.
* *v1.2.0 (September 2025)*: Supported R5 version of FHIR.
* *v1.1.2 (June 2025)*: Added explanations about *PractitionerRole* resource type.
* *v1.1.1 (June 2025)*: Added explanations about *Practitioner* resource type.
* *v1.1.0 (June 2025)*: Added explanations about *Workflow* resource types.
* *v1.0.1 (June 2025)*: Added an example of a `Location` resource type.
* *v1.0.0 (June 2025)*: Documents updated.
* *v0.0.1 (June 2025)*: Completed FHIR server running test. 

&nbsp;

&nbsp;


## Quick Starts 🚀
You can quickly start the FHIR server using the command below, and access it at `localhost:8080` once it's running.
```bash
docker pull hapiproject/hapi:latest
docker run --name fhir_server -p 8080:8080 hapiproject/hapi:latest
```
> [!NOTE]
> If you want to store data locally by mounting PostgreSQL, use the Docker Compose method described below in the "Running a FHIR server" section.

&nbsp;

&nbsp;


## Tutorials & Documentations 🔍
1. [Running a FHIR server](./docs/1_fhir_server.md)
2. [FHIR resources](./docs/2_fhir_resources.md)

&nbsp;

&nbsp;


## References 📚
* [FHIR official documents](https://hl7.org/fhir/)
* [Low level FHIR project](https://github.com/hapifhir/hapi-fhir)
* [FHIR Docker server](https://github.com/hapifhir/hapi-fhir-jpaserver-starter)