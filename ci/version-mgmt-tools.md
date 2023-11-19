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
- one push to release branch = one new release. The tool does not provide a way to control when a new version is created. Can lead to the creation of a lot versions when you would prefer grouping multiple PRs changes into one single new version.
  - mitigation: there is probably a way to work around that linmitation by first merging the PRs in an *intermediate* branch, before merging into the release branch
- change log is pretty dry, because relying entirely on commit messages. No way to add context to the change log. Bad commit messages = bad change log.
