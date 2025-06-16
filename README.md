# FHIR
In this repository, we introduce how to operate FHIR server in the local.
Since we can also integrate PostgreSQL with a local FHIR server, medical data can be stored locally and reloaded automatically when the FHIR server restarts by simply mounting the database volume in Docker.

&nbsp;

### Recent updates 📣
* *June 2025 (v0.0.1)*: Complete FHIR server running test. 

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

<!-- ## Bug Reports
If an error occurs while executing the code, check if any of the cases below apply.
* [Bug Cases](./docs/bugs.md) -->


## References 📚
* [FHIR official documents](https://hl7.org/fhir/)
* [Low level FHIR project](https://github.com/hapifhir/hapi-fhir)
* [FHIR Docker server](https://github.com/hapifhir/hapi-fhir-jpaserver-starter)