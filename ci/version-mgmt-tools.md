# Comparison of version mamagement tools

Evaluated tools:
- https://github.com/semantic-release/semantic-release
- https://github.com/changesets/changesets

Other tools:
- https://github.com/googleapis/release-please

## semantic-release

### How does it work ?
- github action is triggered on push to release branch
- tool determines version bump based on commit messages (https://www.conventionalcommits.org/)
- tool creates new git tag, builds release notes, publishes packages, notifies stakeholders
- configuration through rc files checked in repo

### Pros
- does not require installation of new tools on developer workstation
- integration with github is very good out of the box
- simple solution
- easy to implement
- plugin architecture extending the capabilities of the tool, e.g. publish image to docker registry, send notification in slack, customize changes log...
- very popular
  

### Cons
- developers have to enforce [Conventional Commits](https://www.conventionalcommits.org/). May be fine with you and your team, but harder for outside contributors
  - mitigation: if you are squash-merging feature branches to release branch, the merger has the control over the merge commit message and the contributor commit message won't have any importance
- one push to release branch = one new release. The tool does not provide a way to control when a new version is created. Can lead to the creation of a lot versions when you would prefer grouping multiple PRs changes into one single new version
  - mitigation: there is probably a way to work around that linmitation by first merging the PRs in an *intermediate* branch, before merging into the release branch
- change log is pretty dry, because relying entirely on commit messages. No way to add context to the change log. Bad commit messages = bad change log

## Changesets

### How does it work ?
- developer must add a *changeset* file to each commit
- the *changeset* file specifies the version bump and a message written in markdown
- the *changeset* file can describe changes to multiple packages of the same repo
- the Changeset bot is triggered on each PR push and verifies that *changeset* files are present
- when a PR is merged to release branch, a github workflow is triggered to run the Changeset tool that creates a 'Version Package' branch and PR
- the 'Version Package' branch keeps track of all the PRs merged since the last version published, and keeps a changelog updated at each commit in the release branch
- the version is bumped and package releases only when you decided to merge the 'Version Package' PR

### Pros
- support mono-repos, i.e. git repository hosting multiple packages
- you control when a new version is created (by merging the 'Version Package' PR), meaning that you can group multiple PRs into on single version bump
- change logs look really good:
  - they are not based on commit messages, but by the *changeset* files written in markdown
  - if contributors commit messages are ugly and/or don't respect [Conventional Commits](https://www.conventionalcommits.org/), maintainers can create meaningful *changeset* files
- creates and maintains a CHANGELOG.md file

### Cons
- dependent on nodejs: a CLI is provided to write the *changeset* files that requires the installation of nodesjs and npm on the developer workstation. Also, a *package.json* file must be created in your repo
- solution is not trivial (*changeset* files, 'Version Package' branches), requires more work from the project maintainers
- does not create github releases by default
- contributors are not encouraged to provide meaningful and standardized commit messages

## References
- https://www.reddit.com/r/javascript/comments/10chf6k/askjs_publishing_npm_packages_semanticrelease_or/

## Conclusion
- I would consider **Changesets** for open-source nodejs mono-repos.
- Otherwise I would go with **semantic-release**.
