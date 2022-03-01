# GitLab CI Config

GitLab CI configurations for TypeScript and Go.

- golang
  - validate - checks syntax and run tests
  - sonarlint - static code analysis
- typescript
  - validate - checks syntax and run tests
  - sonarlint - static code analysis
  - deplay-docker

My stage and job names always match. This makes it easier to include and modify jobs.

## How to use

Create a `.gitlab-ci.yml` in your project and add references to the CI files. \
Here is an example for a TypeScript project:

```yml
include:
  - project: 4s1/gitlab-ci-config
    file: 'typescript/validate/.gitlab-ci.yml'
  - project: 4s1/gitlab-ci-config
    file: 'typescript/sonarlint/.gitlab-ci.yml'
  - project: 4s1/gitlab-ci-config
    file: 'typescript/deploy-docker/.gitlab-ci.yml'

stages:
  - validate
  - sonarlint
  - deploy-docker
```

For Go it looks similar, but you have to override the `REPO_NAME` variable:

```yml
include:
  - project: 4s1/gitlab-ci-config
    file: 'golang/validate/.gitlab-ci.yml'
  - project: 4s1/gitlab-ci-config
    file: 'golang/sonarlint/.gitlab-ci.yml'

stages:
  - validate
  - sonarlint

validate:
  variables:
    REPO_NAME: https://gitlab.com/<group name>/<project name>
```

You can also override the default settings, e.g. `image` or add additional setting like `before_script`:

```yml
include:
  - project: 4s1/gitlab-ci-config
    file: 'typescript/validate/.gitlab-ci.yml'

stages:
  - validate

validate:
  # https://gitlab.com/4s1/docker/-/blob/main/node/buster/dev/Dockerfile
  image: registry.gitlab.com/4s1/docker/node:14-buster-dev
  before_script:
    - echo Hello World
```
