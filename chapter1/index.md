## Repeatable deployments

One of the primary reasons to progress a deployment through environments is to gain an increasing confidence that you are providing the end user with a working solution. This confidence can be built through testing (both manual and automated), manual sign off, using your own software internally (drinking your own champagne), early releases to test users, or any number of other processes that allow issues to be identified before they impact the final consumers of your deployments.

However, you only gain this confidence if the thing you are deploying to production is as close as possible to thing you have been verifying in non-production.

By embracing repeatable deployments, you can be sure that what your end users use in production is what you have been testing, verifying and gaining confidence in through your non-production environments.

## General deployment concepts

To understand repeatable deployments, we need to understand what a deployment is, and at what point in a typical build and deployment pipeline deployments take place.

### Continuous Integration, Continuous Delivery and Continuous Deployment

Before we dive into the details of how to implement repeatable deployments, we'll look at how this pillar fits into a CI/CD pipeline.

The terms Continuous Integration and Continuous Delivery or Deployment, or CI/CD, are frequently used to describe the progression from source code to publicly accessible application.

For the purpose of this book, we consider Continuous Integration (CI) to be the process of compiling, testing, packaging, and publishing an application as source code is updated. Specifically, in the case of Kubernetes deployments, CI is the process of packaging code as a Docker image and pushing it to a Docker registry, typically by a CI server.

Continuous Delivery and Continuous Deployment (both abbreviated to CD) have subtly different meanings. We treat both terms as an automated series of steps that deliver an application to its destination. The distinction is whether those automated steps deliver the application directly to the end consumer with no manual human intervention or decision making.

Continuous deployments have no human intervention. You have achieved continuous deployments when a commit to your source code is compiled, tested and packaged by your CI server, and then deployed, tested and exposed to end users. The success or failure of each stage of this process is automatic, resulting in a commit-to-consumer workflow.

Continuous delivery involves a human making the decision to progress a deployment to the final consumers. This progression is typically represented as a promotion through a series of environments. A canonical example of environmental progression is to deploy applications to the non-production development and test environments, and finally the production environment. 

A development environment may very well be configured with continuous deployments, where each commit that is successfully built by the CI server is automatically deployed with no human intervention. Once the developers are happy that their changes are suitable for a wider audience, a deployment can be promoted to the test environment.

The test environment is where quality assurance (QA) staff validate changes, product owners ensure functionality meets their requirements, security teams probe for vulnerabilities etc. Once everyone is happy that the changes meet their requirements, a deployment can be promoted to production.

The production environment is the final destination of a deployment, and is where applications are exposed to their final consumers. 

If you read blogs and tweets on the subject of CI/CD, you may be left with the impression that continuous deployment is the holy grail, and continuous delivery is something of a poor substitute. However, what we have learned from most of our customers is that continuous delivery *works for them*. 

So while the majority of the pillars described in this book apply equally well to continuous delivery and continuous deployment, we'll approach them from a continuous delivery point of view.

:::hint
Deployment strategies like microservices are challenging the notion of non-production and production environments. We'll explore this with the Seamless pillar in chapter 7.
:::hint

### What is an environment?

Environments represent the boundaries between copies of individual applications or entire application stacks and their supporting infrastructure. 

The configuration of all environments should be as similar as possible to ensure that an application will behave consistently regardless of which environment it exists in.

Environments typically represent a progression from an initial environment with a high frequency of deployments and low stability through to the final environment with a low frequency of deployments and high stability.

Deployments are progressed through environments to gain an increasing level of confidence that a working solution can be delivered to the final consumer.

The canonical set of environments are called development, test, and production. The table below describes the characteristics of these environments:

| Environment | Description | Deployment Frequency | Stability / Confidence |
|-|-|-|-|
| Development | Used by developers to test individual changes as they are implemented. | High | Low |
| Test | Use by developers and other non-technical staff to validate changes meet requirements. | Medium | Medium |
| Production | Used by customers to consume the publicly available instance of the applications. | Low | High |

Although you are free to have any number of environments with any names, this book will use this set of environments in the examples.

### What is a deployment?

We have talked about deploying "applications" to environments, which is typically how we describe deployments. But to appreciate how repeatable deployments are achieved, we first need to be specific about what we actually deploy.

In Octopus there are three things that can be deployed to an environment:

1. The compiled applications (a Docker image in the case of a Kubernetes deployment) that are configured for a specific environment. Octopus refers to these as packages.
2. The variables, usually with a small subset specific to individual environments, that define how the applications are configured.
3. Scripts and configuration files written inline (i.e. not saved as files in packages) to support or configure the application and its associated infrastructure in an environment.

Octopus represents the actions to be performed as part of a deployment with a deployment process. A deployment process contains a series of steps. These steps are configured with a combination of variables, package references and inline scripts and configuration files.

Octopus then creates a release. The release is a versioned snapshot of the steps, their configuration, the variables, and the package versions that are to be deployed to an environment.

The release is then deployed to an environment. In this way a consistent bundle of packages, variables, scripts and configuration files are promoted from one environment to the next. Only a small subset of environment specific settings vary from one environment to the next.

The core design of Octopus embraces the pillar of repeatable deployments. A static release is deployed to each environment, ensuring that each environment is as close as possible to the others.

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