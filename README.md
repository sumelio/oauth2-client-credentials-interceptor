# Client Credentials Interceptor

This repository contains a source code of an HTTP request interceptor that augments it by passing along OAuth 2.0 Bearer Token captured earlier using the client credentials flow.

## Getting Started

### Prerequisites

For client applications, the following is required:

- [Java 8](https://www.java.com/en/download/)
- [Spring Boot 1.5.x](https://spring.io/projects/spring-boot)
- [Gradle](https://gradle.org/) or [Maven](https://maven.apache.org/)

If you'd like to build the project on your machine, you'll need [Java 8](https://www.java.com/en/download/) and [Gradle](https://gradle.org/).

### Building

Run the following commands to build the project and publish it to the local Maven repository:

```bash
./gradlew
./gradlew publishToMavenLocal
```

### Usage

First, import an interceptor as a dependency into your Spring Boot project:

Gradle:
```gradle
buildscript {
  repositories {
    /*
      If you're pulling the dependency from your local repository, make sure
      the below is defined under "buildscript -> repositories".
    */
    mavenLocal()
  }
}

dependencies {
  compile 'com.scotiabank.oauth2.oauth2-client-credentials-interceptor:1.0.0'
}
```

Maven:
```xml
<dependencies>
  <dependency>
    <groupId>com.scotiabank.oauth2</groupId>
    <artifactId>oauth2-client-credentials-interceptor</artifactId>
    <version>1.0.0</version>
  </dependency>
</dependencies>
```

Next, whenever you need to OAuth credentials to be passed down (and there, interceptor to be applied), you can inject [RestOperations](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/client/RestOperations.html) with qualifier `"client-credentials-authorized-rest-operations"`.

Here's an example:

```java
public class MySecureHttpClient {
  private final RestOperations restOperations;

  public MySecureHttpClient(
    @Autowired
    @Qualifier("client-credentials-authorized-rest-operations")
    RestOperations restOperations) {

    this.restOperations = restOperations;
  }

  public String downloadString(URI uri) {
    // The "Authorization: Bearer ..." request will automatically be added to the request.
    return restOperations.getForObject(uri, String.class);
  }
}
```
## Contributing

Please read [CONTRIBUTING.md](/CONTRIBUTING.md) for details on our code of conduct, and the process for submitting pull requests to us.

## Versioning

We use [SemVer](https://semver.org/) for versioning. For the versions available, look under "Tags" in this repository.

## Authors

- **Yuri Shewchuk** - initial work
- **Ishan Kelkar** - code lead, contributor
- **Joseph Deluca** - contributor
- **Nathan Marks-Forder** - contributor

## License

This project is licensed under the MPL 2.0 license. see the [LICENSE](/LICENSE) file for details.
