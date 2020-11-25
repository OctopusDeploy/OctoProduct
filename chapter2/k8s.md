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