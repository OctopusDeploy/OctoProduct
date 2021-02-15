## Recoverable deployments

The aim of implementing repeatable and verifiable deployments, tested in non-production environments before being released to production, is to identify bugs before they can affect end users. However, some bugs will inevitably find their way to production. When they do, it is important to restore the production environment to a desirable state.

## Rolling back or forward

Recovering from an undesirable deployment means rolling back to a previous good deployment, or rolling forward to a new version that returns the environment to a desirable state.

Either solution is suitable for stateless applications, but when a database is involved, rollbacks must be treated with care.

This is the [advice from the FlyWay project](https://flywaydb.org/documentation/command/undo#important-notes):

> While the idea of undo migrations is nice, unfortunately it sometimes breaks down in practice. As soon as you have destructive changes (drop, delete, truncate, …), you start getting into trouble. And even if you don’t, you end up creating home-made alternatives for restoring backups, which need to be properly tested as well.

[RedGate offers this advice for database rollbacks](https://documentation.red-gate.com/sca/developing-databases/concepts/advanced-concepts/rollbacks/rollback-guidance):

> Rather than investing time and energy into rollback planning, an alternative is to follow an approach that keeps you moving forward.

The blog post [Pitfalls with SQL rollbacks and automated database deployments](https://octopus.com/blog/database-rollbacks-pitfalls) has this advice:

> More often than not, the effort to successfully rollback a deployment far exceeds the effort it would take to push a fix to production.

When deployments involve database changes, I recommended that you roll forward to recover from an undesirable deployment.

## Rolling back

With repeatable deployments, rolling back can be achieved by rerunning a previous deployment. This is possible because the package versions, scripts, and variables are all defined by a repeatable deployment.

Rollbacks are also an explicit feature of several seamless deployment strategies:
* Canary deployments implement rollbacks by redirecting all traffic from the new deployment to the current deployment. 
* Blue/green deployments can rollback a deployment by cutting traffic back to the blue stack. 
* Session draining deployments can redirect new sessions to the current deployment, and optionally kill any sessions in the new deployment.

Rollbacks have the following benefits:
* A deployment issue can be fixed, without writing code, by rolling back to a previous deployment.
* A rollback leaves the system in a known, verified state.
* The time to complete a rollback can be measured in non-production environments.

Rollbacks have the following disadvantages:
* Rollbacks are all or nothing operations. You can not roll back individual features, only entire deployments.
* Rollbacks need to be tested as part of the deployment process to ensure they work as expected, which increases the complexity and time of the deployment process.
* If a rollback fails, it is likely that you will need to resolve the issue by rolling forward.
* Database rollbacks require special consideration to ensure data is not lost.

## Rolling forward

Rolling forward is simply another way to describe performing a new deployment. In this case the new deployment will only contain the fixes required to restore an environment.

Rolling forward has the following benefits:
* All deployments strategies, with or without a database, inherently support rolling forward.
* Teams gain experience in rolling forward with every deployment.
* You can choose the scope of a change or fix when rolling forward.
* Multiple deployments can be made in succession while rolling forward to resolve an undesirable deployment.

Rolling forward has the following disadvantages:
* Rolling forward typically requires a developer to implement a fix to include in the next deployment.
* Rolling forward may involve bypassing the environmental progression typically used to verify a deployment to get a fix deployed as quickly as possible.
* The production environment will be left in an undesirable state for as long as it takes to develop and deploy the next version.

## Links
* [Foreword](../chapter0/index.md)
* [Repeatable deployments](../chapter1/index.md)
* [Verifiable deployments](../chapter2/index.md)
* [Seamless deployments](../chapter3/index.md)
* [Recoverable deployments](../chapter4/index.md)
* [Visible deployments](../chapter5/index.md)
* [Measurable deployments](../chapter6/index.md)
* [Auditable deployments](../chapter7/index.md)
* [Standardized deployments](../chapter8/index.md)
* [Maintainable deployments](../chapter9/index.md)
* [Coordinated deployments](../chapter10/index.md)
