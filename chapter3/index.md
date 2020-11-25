## Seamless deployments

Deploying new versions of your applications inevitably means taking the previous version offline and directing traffic to the new version. Ensuring end users are not adversely affected during this cutover requires some planning.

A logical solution is to perform public facing deployments at a time when end users are unlikely to be using the applications. If your customers are located in a similar timezone, deploying new versions of your application in the middle of the night effectively results in a seamless experience for end users.

Midnight deployments may not glamorous, but if they *work for you*, this is a perfectly valid solution.

When downtime has to be kept to a minimum, or there is no good time for an outage window, some common deployment strategies can be used to seamlessly deploy new application versions.

## Seamless database deployments

No discussion on seamless deployments can begin without first addressing the issues of database updates.

A fundamental aspect of most seamless deployment strategies involves running two versions of your application side by side, if only for a short period of time. If both versions of the application access a shared database, then any updates to the database schema and data must be compatible with both application versions. This is referred to as backwards and forward compatibility.

However, backwards and forward compatibility is not trivial to implement. In the presentation [Update your Database Schema with Zero Downtime Migrations](https://www.youtube.com/watch?v=3mj6Ni7sRN4) (based on chapter 3 of the book [Microservice database migration guide](https://developers.redhat.com/books/migrating-microservice-databases-relational-monolith-distributed-data)) Edison Yanaga walks through the process of renaming a single column in a database. It involves six incremental updates to the database and application code, and all six versions to be deployed sequentially.

Needless to say, seamless deployments involving databases require planning, many small steps to roll out the changes, and tight coordination between the database and the application code.

## Deployment strategies

There are multiple different strategies that can be used to manage the cutover between an existing deployment and a new one.

### Recreate

The recreate strategy is does not provide a seamless deployment, but is included here as it is the default option for most deployment processes. This strategy involves either removing the existing deployment and deploying the new version, or deploying the new version over the top of the existing deployment.

Both options result in downtime between the time when the existing version has been stopped or removed and the new version is started. However, because the existing and new versions are not run concurrently, database upgrades can be applied as needed with no backwards and forward compatibility requirements.

### Rolling updates

The rolling update strategy involves incrementally updating instances of the current deployment with the new deployment. This strategy means that there is always at least one instance of the current or new deployment available during the deployment. This means that any shared database must maintain backwards and forward compatibility.

### Canary deployments

The canary strategy is similar to the rolling update strategy in that both strategies incrementally expose more end users to the new deployment. The difference is that the decision to progress the rollout in a canary deployment is either made automatically by a system monitoring metrics and logs to ensure the new deployment is performing as expected, or manually by a human.

Canary deployments also have the option to halt the rollout and revert back to the existing deployment if a problem was discovered.

### Blue/green deployments

The blue/green strategy involves deploying the new version (referred to as the green version) alongside the current version (referred to as the blue version), without exposing the green version to any traffic. Once the green version is deployed and verified, traffic is cutover from the blue to the green version. Once the blue version is no longer used, it can be removed.

Any database changes deployed by the green version must maintain backwards and forward compatibility, because even if the green version is not serving traffic, the blue version will be exposed to the database changes.

### Session draining

The session draining strategy is used when the application maintains state tied to a particular application version.

This strategy is similar to the blue/green strategy in that both will deploy the new version alongside the current version, and run both side by side. Unlike the blue/green strategy, which will cut all traffic over to the new deployment in a single step, the session draining strategy will direct new sessions to the new deployment, while the existing deployment serves traffic to existing sessions.

Because the current and new deployments run side by side, any database changes must maintain backwards and forward compatibility.

Once the old sessions have expired, the existing deployment can be deleted.

### Feature flags

The feature flag strategy involves building functionality into a new application version, and then enabling or disabling the feature for select end users outside of the deployment process.

In practice the deployment of a new application version with flaggable features will be performed with one of the strategies above, so the feature flag strategy is a complement to those other strategies.

### Feature branch

The feature branch strategy allows developers to deploy an application version with changes they are currently implementing, usually in a non-production environment, alongside the shared deployment.

It many not be necessary to maintain database backwards and forward compatibility with feature branch deployments. Because feature branches are for testing and tend to be short lived, it may be acceptable that each feature branch deployment have access to its own test database.

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