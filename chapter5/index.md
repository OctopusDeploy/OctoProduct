## Visible deployments

Deployments are an abstract concept, and it is difficult to know what has been deployed where by inspecting the state of a system. Determining what versions of an application are installed from the files on a disk or the records in a database is like working out the mix of colors that were used to produce a tin of paint.

Being able to quickly view the current state of your environments is critical to understanding: 

* What features have been provided to your customers.
* What features are being tested. 
* What issues have been fixed.
* The history of any changes.

Listed below are a number of details required to gain full visibility into the state of your deployments.

## Commit messages

Commit messages capture the intention of source code edits, describing what changes were made and by whom. These messages can be invaluable when trying to understand at a low level what changes made it into a particular version of a package.

## Issue tracking

Often source code commits are made to resolve an issue documented in a dedicated issue tracker. These issues provide a space for bugs to be described, discussed and tracked. Each issue is referenced by a unique identifier.

Capturing the issue IDs that relate to changes in a package version, and any deployment that includes that package version, provides insight into the issues that were resolved in any given deployment.

## CI build logs

A typical CI/CD pipeline will have a CI server that builds, tests and packages an application. The log files for these builds contain a wealth of information such as which tests passed, which tests were ignored, which dependencies were included, and what packages were created. A link to the CI build from the deployment allows these log files to be quickly reviewed.

## Library dependencies

Almost every application deployed today is a combination of custom code and shared libraries, often provided by a third party. These libraries can be a source of bugs or security vulnerabilities, or provide new and useful features. Understanding the library dependencies that contributed to a deployment is important for auditing and debugging.

## Deployment versions

A release captures all of the above information, along with package versions, variable values and scripts, in a release version. This release is then deployed to an environment.

Displaying which release versions are deployed to which environments provides a high level view of the state of your deployments. With this information anyone can see what was deployed where, and by drilling into the details of a release, can see the commit messages, issues, dependencies and links to the CI builds.

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
