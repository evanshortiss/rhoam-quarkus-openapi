# Quarkus OpenAPI and Red Hat OpenShift API Management

This sample application can be used to demonstrate deploying a Quarkus,
application on OpenShift, and the Service Discovery capability of 3scale.

Specifically, it has been designed and tested on OpenShift Dedicated and Red
Hat OpenShift API Management.

This application is a modified version of the Quarkus quickstart/guide for
[OpenAPI and Swagger UI](https://quarkus.io/guides/openapi-swaggerui)*. Notable
differences:

* Set `quarkus.smallrye-openapi.store-schema-directory` in *application.properties*
* Updated *.gitignore* with `openapi.json` and `openapi.yaml`
* Added the `quarkus-openshift` extension

## Usage

Verify that your system meets the following requirements:

* JDK 8 or 11+
* Gradle or Apache Maven 3.6.2+

### Dev Mode

Exposes the application on `http://localhost:8080` and enables live reload.

```bash
./mvn quarkus:dev
```

### Deploy to OpenShift

*NOTE: This requires the OpenShift (`oc`) Command-Line tool to be installed on your system, and that you have a valid cluster login.*

```bash
./mvnw clean package -Dquarkus.kubernetes.deploy=true -Dquarkus.openshift.expose=true
```

### Import to 3scale API Management

Add the following annotations and label to the Service associated with the Quarkus application you deployed in the previous section:

```bash
oc annotate svc/rhoam-openapi "discovery.3scale.net/description-path=/openapi?format=json"
oc annotate svc/rhoam-openapi discovery.3scale.net/port="8080"
oc annotate svc/rhoam-openapi discovery.3scale.net/scheme=http
oc label svc/rhoam-openapi discovery.3scale.net="true"
```

Now you can import the Service using 3scale Service Discovery.
