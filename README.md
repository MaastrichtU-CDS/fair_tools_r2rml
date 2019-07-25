# R2RML
This repository contains a docker image generation for the R2RML implementation available at [https://github.com/jvsoest/r2rml](https://github.com/jvsoest/r2rml).

This container is being used in the [DataFAIRifier](https://github.com/maastroclinic/DataFAIRifier) project as a back-end service container.

## Configuration for debugging
Please run the following line for the given operating system.

### Windows
```
echo. 2>output.ttl

docker run --rm -it ^
    -e DB_JDBC=jdbc:postgresql://172.17.0.1:2345/mydata ^
    -v %cd%\mapping.ttl:/mapping.ttl ^
    -v %cd%\output.ttl:/output.ttl ^
    jvsoest/r2rml bash run.sh --debug
```

### Linux/macOS/Unix:
```
echo "" > output.ttl

docker run --rm -it \
    -e DB_JDBC=jdbc:postgresql://172.17.0.1:2345/mydata \
    -v $(pwd)/mapping.ttl:/mapping.ttl \
    -v $(pwd)/output.ttl:/output.ttl \
    jvsoest/r2rml bash run.sh --debug
```

## Configuration within DataFAIRifier
Within a docker-compose file, the following service can be added (minimal configuration):
```
  r2rml:
    image: registry.gitlab.com/um-cds/fair/tools/r2rml:1.0
    links:
      - postgres:dbhost
      - graphdb:graphdb
```

The following environment variables are optional to include:
* SLEEPTIME: The time in between execution runs (in seconds)
* DB_JDBC: A full jdbc connection string
* DB_USER: username for the database connection
* DB_PASS: password for the database connection
* BASE_IRI: base URL for the generated RDF triples
* R2RML_ENDPOINT: endpoint location for the SPARQL endpoint where the R2RML statements are stored
* OUTPUT_ENDPOINT: endpoint location for the SPARQL endpoint where the resulting statements are stored
* GRAPH_NAME: graph name (otherwise called context name) within the OUTPUT_ENDPOINT where the resulting statements are stored