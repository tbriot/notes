Original article: [Our Software Dependency Problem](https://research.swtch.com/deps), by Russ Cox

Also published under the title "Surviving Software Dependencies" [here](https://queue.acm.org/detail.cfm?id=3344149) and [here](https://dl.acm.org/doi/pdf/10.1145/3347446).

Note: this article uses the term *package* instead of library or module to refer to software dependency.

## Context
- with new package managers it has never been easier to install deps (npm, apt, pip..). Almost no cost at first glance.
- rise of open-sourced softwaree = anyone can publish a package
- good thing: finally developers can reuse code, and not reinvent the wheel
- **dependency risk** is too often **overlooked**
- aim of the article is to raise awareness to the risk and provide good practices to manage software dependencies, at a high-level.

## What could go wrong ?
- adding a dependency = **outsourcing** the work of developing a piece of code: designing, writing, testing, debugging and maintaining.
- a dependency can be acceptable for a personal hobby project but not for a commercialized product.

## Inspect the dependency
- choosing a package should be **similar to hiring a new developer**: check references, popularity, testing. Like a job interview.
- make sure the package has a comprehensive tests set. Better testing = mitigating the risk of a regresssion.
- check usage: how many applications or packages depend on this package ? More people depends on the package = the better. More people to catch bugs, more people to hop in and make sure the package keeps being maintained.
- check the **license** of the package !
- **transitive dependencies** !

## Test the dependency
- write your own tests against the package API, validate interfaces your project use
- performance testing
- security scans

## Abstract the dependency
- thin wrappers make it easier to switch later to another package

## Avoid the dependency
- if too much risk and the package is not that big, you can **copy** the dependency code in your own codebase (if the license allows it)
-  “A little copying is better than a little dependency.”

## Upgrade the dependency
- as often as possible
- test the new version of the package
- security fix must be integrated ASAP. E.g. Equifax 2017 catastrophe.
