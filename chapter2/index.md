## Verifiable deployments

The repeatable deployments pillar describes how promoting deployments through environments provided an increasing level of confidence that the solution delivered to the final consumer met all the requirements. We talked about how frequent deployments to the development environment enabled developers to test their changes, while less frequent deployments to the test environment allowed other parties to verify any changes. Once everyone was happy, the production environment is updated, exposing the changes to end users.

In this chapter we'll explore the pillar of verifiable deployments, looking at the various techniques that can be used to verify a deployment once it has reached a new environment.

## General testing concepts

Testing is a nebulous term with often ill-defined subcategories. We will not attempt to provide authoritative definitions of testing practices here. Our goal is to offer a very high level description of common testing practices, highlighting those that can be performed during the deployment process.

### What do we not test during deployments?

Let's start with the kind of tests that are not performed as part of the deployment process.

Unit tests are considered part of the build pipeline. These tests are tightly coupled to the code being compiled, and must succeed for the resulting application package to be built. 

Integration tests may also be run by the CI server to verify that higher level components interact as expected. The components under test may be replaced with a test double to make the tests more reliable, or live instances of the components may be created as part of the test.

Unit and integration tests are run by the CI server, and any package that is made available for deployment is assumed to have passed all its associated tests unit and integration tests.

:::hint
**Concept explanation: Test double**

[Test double](https://martinfowler.com/bliki/TestDouble.html) is a generic term for any case where you replace a production object for testing purposes. 
:::

### What can we test during deployment?

Tests that require a live application or application stack to be accessible are ideal candidates to be included in a deployment process.

Smoke tests are quick tests designed to ensure that applications and services have deployed correctly. Smoke tests implement the minimum interaction required to ensure services are responding. Some examples include:

* A HTTP request of a web application or service to check for a successful response.
* A database login to ensure the database is available.
* Checking that a directory has been populated with some files.
* Querying the infrastructure layer to ensure the expected resources were created.

Integration tests can be performed as part of a deployment as well as during the build. Some of the components being verified may be test doubles included as part of the deployment, or the tests may verify two or more live component instances. Integration tests validate that multiple components are interacting as you expect, and like smoke tests, implement the minimum interaction required to get a result. Some examples include:

* Logging into a web application to verify that it can interact with an authentication provider.
* Querying an API for results from a database to ensure that the database is accessible via a service.

Functional tests are like integration tests, but verify a broader range of inputs and outputs. Some examples include:

* Logging into a web application with a known valid user, and verifying the operation to succeeded.
* Logging into a web application with a known invalid user, and expecting the operation to fail.
* Requesting an entity that is known to exist through an API, and verifying the returned data.
* Requesting an entity that is known not to exist through an API, and verifying the returned error.

End to end tests provide an automated way of interacting with a system in the same way a user would. These can be long running tests following paths through the application that require most or all components of the application stack to be working correctly. Examples include:

* Automating the interaction with a online store web application to browse an online catalogue, view an item, add it to a cart, complete the checkout, and review the account order history.
* Completing a series of API calls to a weather service to convert a city to a latitude and longitude, getting the current weather for the returned location, and returning the forecast for the rest of the week.

Chaos testing involves deliberately removing or interfering with the components that make up an application to validate that the system is resilient enough to withstand such failures. Chaos testing may be combined with the other types of tests to verify the stability of a degraded system.

Usability and acceptance testing typically require a human to use the application to verify that it meets their requirements. The requirements can be subjective, for example determining if the application is visually appealing. Or the testers may not be technical, and so do not have the option of automating the tests. The manual and subjective nature of these kinds of tests makes them difficult, if not impossible, to automate, meaning a working copy of the application or application stack must be deployed and made accessible to testers.

## Modelling tests in Kubernetes and Octopus

Now that we understand the types of tests that are performed during the deployment process, we can map them to specific implementations in Kuberenets and Octopus.

### Modelling tests in Kubernetes

Tests run against deployments in Kubernetes can either be external or internal.

External tests are executed outside of the cluster, and interact with publicly exposed resources. Usability and acceptance testing are good examples of external tests, as they will be performed in the testers browser. Externally managed testing platforms like BrowserStack, Sauce Labs, New Relic, Gatling etc. also access your deployments externally.

Internal tests are executed as pods inside the cluster, and can interact with both internally and externally exposed resources. Internal tests are required for smoke and integration tests that access pods that have no public service. For example, a database hosted in Kubernetes may not expose a public port, but could be tested by code running inside the cluster.

Kubernetes jobs support running and monitoring short lived pods, making them a good option for running internal tests. Jobs can be created in the same namespace as the services being tested, giving them easy access to cluster IP services.

### Modelling tests in Octopus

Tests in Octopus are steps run after the main components of a deployment have completed.

External tests will typically be run from workers that have access to the publicly exposed Kubernetes resources. The workers can execute scripts designed to test these public resources, returning a zero exit code if the tests succeed or a non-zero exit code if the tests fail.

:::hint
**Concept explanation: Exit codes**

Executables in all common operating systems return an integer when they complete, called an exit code, to indicate if the executable was successful or not. An exit code of zero indicates success. A non-zero exit code indicates a failure.

Octopus script steps inspect the exit code of the last completed command to determine if the step was a success or not.
:::

Internal tests are executed by creating a new job resource in the cluster and waiting for it to complete. The status of the job can then be inspected to determine if the tests were successful or not.

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