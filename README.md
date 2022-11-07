# Software and IaC deployment blueprint

* Aspects:
  * Software components (WHL, JAR, Docker Image, etc):
    * Pre-build test(s)
    * Build artifact
    * Post-build test(s)
    * Deploy artifact to artifact store (e.g. maven central, ECR, etc)
  * IaC components:
    * Platform
    * Application

# Introduction

## Build then tag, or tag then build from tag?

A disadvantage of building from a tag is that it would not run on an arbitrary
commit to a feature branch. Commits to feature branches should not generate
releases; only commits to main (or other designated release branches) should
generate releases.

Also, tagging before a build (and then building from the tag) might result in
an invalid release tag being left in the repository, which could easily be
mistaken for a valid release in the future (if the tag is not removed).

# Software Components

* Typically expect one component per git repository
  * Might multiple work if they share the same version number and git tag?
* Use main branch and feature branches for development
* Use main branch to run tests, test builds and also generate releases
* Use feature branch to run tests and test builds only (no releases)

```
# = commit
        pre-test, build, post-test; tag, release
                   |               |
--- main --+-------#---------------#---
            \                     /
             +--- feature ---#---+
                             |
                  pre-test, build, post-test
```

# IaC (Terraform)
