# FHIR Server
Here, we introduce how to run a FHIR server locally.
> Based on [HAPI-FHIR](https://github.com/hapifhir/hapi-fhir-jpaserver-starter) (as of 2025-06-11).

&nbsp;

## Running a FHIR Serever
### 1. Runnig via Docker
You can simply run a FHIR server via Dokcer commands:
```bash
docker pull hapiproject/hapi:latest

# Base execution
docker run --name ${CONTAINER_NAME} -p 8080:8080 hapiproject/hapi:latest     # You can change the ports number

# Or you can run a Docker container at your backend
docker run -it -d --name --name ${CONTAINER_NAME} -p 8080:8080 hapiproject/hapi:latest     # You can change the ports number
docker logs -f --tail 1000 ${CONTAINER_NAME}     # You can trace logs via this command
```

&nbsp;


### 2. Running via `docker-compose.yml`
You should first complete the `docker-compose.yml`.

<details>
<summary><code>docker-compose.yml</code></summary>

You should set `${CONTAINER_NAME}` in the yaml file.
```yaml
version: '3.7'

services:
  fhir:
    container_name: ${CONTAINER_NAME1}
    image: "hapiproject/hapi:latest"
    ports:
      - "8080:8080"
    configs:
      - source: hapi
        target: /app/config/application.yaml
    depends_on:
      - db
  db:
    container_name: ${CONTAINER_NAME2}
    image: postgres
    restart: always
    environment:
      POSTGRES_PASSWORD: admin
      POSTGRES_USER: admin
      POSTGRES_DB: hapi
    volumes:
      - ./postgress.data:/var/lib/postgresql/data

configs:
  hapi:
     file: ./hapi.application.yaml
```
</details>

<br>After completing the `docker-compose.yml` file, you can run a FHIR server using it:
```bash
# Base execution
docker compose -f docker-compose.yml up --build     # If you alread have image, you don't need '--build' option

# Or you can run a docker-compose at your backend
docker compose -f docker-compose.yml up --build -d    # If you alread have image, you don't need '--build' option
docker compose -f docker-compose.yml logs -f --tail 1000    # You can trace logs via this command
docker compose -f docker-compose.yml down   # You can finish your docker-compose using this command

```

&nbsp;