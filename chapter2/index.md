## Verifiable deployments

The repeatable deployments pillar describes how promoting releases through environments provides an increasing level of confidence in the solution being delivered. We talked about how frequent deployments to the development environment enabled developers to test their changes, while less frequent deployments to the test environment allowed other parties to verify a potential production release. Once everyone was happy, the production environment is updated, exposing the changes to end users.

The pillar of verifiable deployments describes the various techniques that can be used to verify a deployment once it has reached a new environment.

## General testing concepts

Testing is a nebulous term with often ill-defined subcategories. We will not attempt to provide authoritative definitions of testing categories here. Our goal is to offer a very high level description of common testing practices, highlighting those that can be performed during the deployment process.

### What do we not test during deployments?

Unit tests are considered part of the build pipeline. These tests are tightly coupled to the code being compiled, and must succeed for the resulting application package to be built. 

Integration tests may also be run during the build process verify that higher level components interact as expected. The components under test may be replaced with a [test double](https://martinfowler.com/bliki/TestDouble.html) to improve reliability, or live instances of the components may be created as part of the test.

Unit and integration tests are run by the CI server, and any package that is made available for deployment is assumed to have passed all its associated unit and integration tests.

### What can we test during deployment?

Tests that require a live application or application stack to be accessible are ideal candidates to be run as part of a deployment process.

Smoke tests are quick tests designed to ensure that applications and services have deployed correctly. Smoke tests implement the minimum interaction required to ensure services respond correctly. Some examples include:

* A HTTP request of a web application or service to check for a successful response.
* A database login to ensure the database is available.
* Checking that a directory has been populated with files.
* Querying the infrastructure layer to ensure the expected resources were created.

Integration tests can be performed as part of a deployment as well as during the build. Integration tests validate that multiple components are interacting as you expect. Test doubles may be included with the deployment to stand in for components being verified, or the tests may verify two or more live component instances. Examples include:

* Logging into a web application to verify that it can interact with an authentication provider.
* Querying an API for results from a database to ensure that the database is accessible via a service.

End to end tests provide an automated way of interacting with a system in the same way a user would. These can be long running tests following paths through the application that require most or all components of the application stack to be working correctly. Examples include:

* Automating the interaction with a online store to browse a catalogue, view an item, add it to a cart, complete the checkout, and review the account order history.
* Completing a series of API calls to a weather service to find a city's latitude and longitude, getting the current weather for the returned location, and returning the forecast for the rest of the week.

Chaos testing involves deliberately removing or interfering with the components that make up an application to validate that the system is resilient enough to withstand such failures. Chaos testing may be combined with other tests to verify the stability of a degraded system.

Usability and acceptance testing typically require a human to use the application to verify that it meets their requirements. The requirements can be subjective, for example determining if the application is visually appealing. Or the testers may not be technical, and so do not have the option of automating tests. The manual and subjective nature of these tests makes them difficult, if not impossible, to automate, meaning a working copy of the application or application stack must be deployed and made accessible to testers.

## Links
* [Foreward](../chapter0/index.md)
* [Repeatable deployments](../chapter1/index.md)
* [Verifiable deployments](../chapter2/index.md)
* [Seamless deployments](../chapter3/index.md)
* [Recoverable deployments](../chapter4/index.md)
* [Visible deployments](../chapter5/index.md)
* [Measureable deployments](../chapter6/index.md)
* [Auditable deployments](../chapter7/index.md)
* [Standardized deployments](../chapter8/index.md)
* [Maintainable deployments](../chapter9/index.md)
* [Coordinated deployments](../chapter10/index.md)