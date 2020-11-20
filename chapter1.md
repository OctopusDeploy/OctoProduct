## Repeatable deployments

## Continuous Integration, Continuous Delivery and Continuous Deployment

The terms Continuous Integration and Continuous Delivery/Deployments, or CI/CD, are frequently used to describe the progression from source code to publicly accessible application.

For the purpose of this book, we consider Continuous Integration (CI) to be the process of compiling, testing and packaging an application as source code is updated. Specifically, in the case of Kubernetes deployments, CI is the process of packaging code as a Docker image, typically by a CI server.

Continuous Delivery and Continuous Deployments (both abbreviated to CD) have subtly different meanings. We treat both terms as an automated series of steps that deliver an application to its destination. The distinction is whether those automated steps deliver the application directly to the end consumer with no manual human intervention or decision making.

Continuous deployments have no manual human intervention. You have achieved Continuous Deployments when a commit to your source code is compiled, tested and packaged by your CI server, and then deployed, tested and exposed to end users. Each stage of this process is automatic, resulting in a commit-to-consumer workflow.

Continuous delivery involves a human making the decision to progress a deployment to eventual consumers. Typically this progression is represented as a series of environments. A canonical example of environmental progression is to deploy applications to development, test and production environments. 

A development environment may very well be configured with continuous deployments, where each commit that is successfully built by the CI server is automatically deployed with no human intervention. Once the developers are happy that their changes are suitable for a wider audience, a deployment can be promoted to the test environment.

The test environment is typically where quality assurance (QA) staff validate changes, product owners ensure functionality meets their requirements, security teams probe for vulnerabilities etc. Once everyone is happy that the changes meet their requirements, a deployment can be promoted to production.

The production environment is the final destination of a deployment, and is where applications are exposed to their final consumers. 

If you read blogs and tweets on the subject of CI/CD, you may get the impression that continuous deployments are the holy grail, and with continuous delivery being something of a poor substitute. What we have learned from most of our customers is that continuous delivery *works for them*. 

The majority of the pillars described in this book apply equally well to continuous delivery and continuous deployments, but we'll approach them from a continuous delivery point of view.

:::hint
Deployment strategies like microservices are challenging the canonical notion of non-production and production environments. We'll explore this with the Seamless pillar in chapter 7.
:::hint

## What is a deployment?

In the previous section we talked about deploying "applications" to environments, which is typically how we talk about deployments. But to appreciate how to achieve repeatable deployments, we first need to be more specific about what we actually deploy.

There are typically three things that are deployed to an environment:

1. The compiled applications (a Docker image in the case of a Kubernetes deployment) that are configured within an environment. Octopus refers to these as packages.
2. The variables, usually with a small subset specific to some environments, that define how the applications are configured.
3. Scripts and configuration files written inline (i.e. not saved as files in packages) to support or define the application in an environment.

Octopus represents a deployment as a series of steps. These steps are configured with variables, reference packages and optionally defining inline scripts and configuration files.

Octopus then creates a release. The release is a snapshot of the steps, their configuration, the variables, and the package versions that are to be deployed to an environment.

The release is then deployed to an environment. In this way a consistent bundle of packages, variables, scripts and configuration files are promoted from one environment to the next. Only a small subset of environment specific settings are changed from one environment to the next.

The core design of Octopus embraces the pillar of repeatable deployments. The information contained in a release is deployed to each environment, ensuring that each environment is as close as possible to the others.

## Benefits of repeatable deployments

One of the primary reasons to progress a deployment through environments is to gain an increasing confidence that you are providing the end user with a working solution. This confidence can be earned though testing (both manual and automated), manual sign off, using your own software internally (drinking your own champagne), early releases to test end users, or any number of other processes that allow issues to be identified before they impact end users.

However you only gain this confidence if the thing you are deploying to production is as close as possible to thing you have been verifying in non-production.

By embracing repeatable deployments, you can be sure that what your end users see in production is what you have been testing, verifying and gaining confidence in through your non-production environments.

## Modelling environments with Kubernetes

There are two common ways to model environments in Kubernetes.

The first way to model environments is to use namespaces to partition a single cluster into many logical environments. 

A simple approach would be to create namespaces like `development`, `test`, and `production`, which releases being deployed inside each namespace. While easy to implement, this option does limit the ability to use Role Based Access Controls (RBAC) rules as a way of preventing rouge deployments from overwriting or deleting they shouldn't within an environment. Because RBAC rules apply to all resources of a certain type within a namespace (for example by allowing a specific Kubernetes user to create, update or delete pods) it is possible that a mistyped pod name in a deployment will overwrite an unrelated pod in the same namespace.

This limitation can be overcome by having separate namespaces for each combination of deployment and environment. For example, you may have six namespaces to contain the deployments for a frontend and backend application called `development-frontend`, `development-backend`, `test-frontend`, `test-backend`, `production-frontend`, `production-backend`. Six Kubernetes accounts could then be used to perform each deployment to each environment, with each account only having access to their intended namespace.

Even when using fine grained namespaces to separate deployments, there are cases where Kubernetes resources can not be isolated. A classic example of this are Custom Resource Definitions (CRDs). CRDs are scoped to a cluster, and so can not be restricted to individual namespaces. It is also usually impractical to implement network bandwidth limits per namespace.

Kubernetes does not provide a hard tenancy model where multiple untrusted tenants, or environments in our case, can operate on a single cluster with guarantees that one tenant will not affect another. Namespaces go a long way to providing multi-tenancy within a single cluster, but do not provide a complete solution.

The second way to model environments is with multiple clusters. Individual clusters provide a high degree of separation between environments, isolating shared resources like network traffic, cluster wide resources like CRDs, and node CPU and memory.

More likely you will use a combination of the two approaches by having a single cluster for the development and test environments, and a separate cluster for production.

## Kubernetes account types

To interact with Kubernetes resources, we need to authenticate with an account. [Kubernetes distinguishes between users accounts, which represent a human, and service accounts, which represent a machine](https://kubernetes.io/docs/reference/access-authn-authz/service-accounts-admin/#user-accounts-versus-service-accounts). Octopus supports both types of accounts when connecting to a cluster, but using a service account is recommended.

## Restricting Kubernetes access

RBAC is implemented in Kubernetes through roles and role bindings. Specifically, Kubernetes has four resources to define RBAC rules:

* Role: a namespace scoped resource that defines the allowed operations on resources in the role's namespace.
* RoleBinding: a namespace scoped resource that maps a role in role binding's namespace to a user or service account.
* ClusterRole: a cluster scoped resource that defines the allowed operations resources in all namespaces.
* ClusterRoleBinding: a cluster scoped resource that maps a cluster role to a user or service account.

In general terms, roles grant access to Kubernetes resources, and role bindings link accounts to roles.

The recommended strategy is to have service accounts with the minimum level of permissions required to complete a single deployment to an environment. This provides a guarantee that a deployment to one environment, such as the development environment, can not accidentally overwrite resources in another environment, such as the test or production environments.

## Modelling Kubernetes environments in Octopus

Taken together, the combination of a cluster, account and namespace represent a security boundary into which a deployment can be performed. In Octopus, this security boundary is represented as a Kubernetes target.

The Kubernetes target is the glue between the physical (i.e. separated clusters) or logical (i.e. separate namespaces) Kubernetes environments, the Octopus environments, and individual deployments that take place in those environments, which Octopus models as roles.

## The sample deployment

To demonstrate repeatable deployments, we'll deploy a sample application with a frontend and backend component to the development, test, and production environments. We'll create the development and test environments in one Kubernetes cluster, and the production environment in a second Kubernetes cluster.

## Concepts overview

The following table matches general concepts to their matching implementations in both Kubernetes and Octopus:

| General Concept | In Kubernetes | In Octopus |
|-|-|-|
| Environments | A cluster or namespace | Environment |
| Account | Service account | Token account |
| | Username and password account | Username/password account |
| | Certificate account | Certificate |
| Deployment category | May be part of the namespace e.g. `frontend` in the `development-frontend` namespace.  | Role |
| Security boundary | Cluster, namespace and account | Kubernetes target |



## Conclusion