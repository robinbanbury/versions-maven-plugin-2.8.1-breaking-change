# Breaking change in Versions Maven Plugin v2.8.1

Version 2.8.1 of the Versions Maven Plugin is incompatible with projects containing a parent POM (e.g. a Bill of Materials POM) in a project subdirectory.

## Replication steps

Clone this repo. Confirm the code builds by running `mvn clean verify`.

### Test 1

Run the following command:

```mvn versions:set -DnewVersion=2.0.0-SNAPSHOT --file product-bom/pom.xml```

Only the project version in `product-bom/pom.xml` is updated. This breaks the project structure, as the test-product POM and product-bom POM version numbers no longer match. Confirm this is the case by running the build again `mvn clean verify` - it should fail.

### Test 2

Reset any file changes. Run the following command:

```mvn versions:set -DnewVersion=2.0.0-SNAPSHOT```

This will fail with the following message: `[ERROR] Failed to execute goal org.codehaus.mojo:versions-maven-plugin:2.8.1:set (default-cli) on project test-product: Project version is inherited from parent. -> [Help 1]`

## Comparison with 2.7

Using version 2.7 of the Versions Maven Plugin, the project version can be updated throughout the project POMs, using the following command: `mvn org.codehaus.mojo:versions-maven-plugin:2.7:set -DnewVersion=2.0.0-SNAPSHOT --file product-bom/pom.xml`.

Confirm the project still builds using `mvn clean verify`.
