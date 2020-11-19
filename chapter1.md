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