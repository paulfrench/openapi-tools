# Setup

Clone this project and rename it to whatever you require.

Make sure npm is installed on your machine

Install opena-api-generator-cli...

```
npm install @openapitools/openapi-generator-cli -g
```

# Generate Spring Boot Server Project from OpenAPI v2/3 Spec

Edit the gen-server-stubs.bat file and replace the example URL with the location of your API spec (this can be a file location or URL)

`NOTE:` for Corporate SwaggerHub API simply take the URL to your API definition on SwaggerHub and replace `https://app` part with `https://api` to get a URL reference to the fully resolved OpenAPI spec.

`BUG ALERT:` (version 5.1.1) since the open api generator (for the java language) does not support `allOf`, if your schema specifies `allOf` in any response then either move this definition to the schema section and reference the single schema from there or use the swaggerhub export tool to export your api where it will automatically do this for you.

In a terminal from the `scripts` folder run the `gen-server-stubs.bat` file

Refresh the project and view in your favourite IDE. You may also need to tell the IDE to convert to a Maven project so that it reads the pom.xml and structures the project as required.

Open the `.openapi-generator-ignore` file and comment in the 2 entries. This ensures that if the API spec changes you can re-generate the API code and any changes you have made to the `pom.xml` or `OpenAPI2SpringBoot.java` are not overwritten.

Your main entry point by default is...

```
org.openapitools.OpenAPI2SpringBoot.java
```

The generated project uses the delegate pattern, so start to implement the API interfaces and mark the class with the Spring annotation `@Service`. These delegates will get picked up and injected into the controllers. You will need to edit the `OpenAPI2SpringBoot.java` file and add any new packages that require scanning by Spring.

The application.properties file that gets created allows you to make some config changes, for example I like to omit null fields from any response and like to change the base path

```
openapi.XXX.base-path=/bce
spring.jackson.default-property-inclusion = NON_NULL
```

For the value of `openapi.XXX.base-path` look at one of the generated controllers to see what the property is called, it does depend on your OAS spec.


## Gotcha for JAVA

* Do not use allOf in the API spec when specifying a response. Specify allOf in the schema definitions and reference the single schema from the response (for some reason the open api generator will only use the first schema specified in the allOf section if specified in the response)

# Generate Java Client Project from OpenAPI v2/3 Spec




