# GitLab CI/CD
You can use GitLab CI/CD to automatically build and test your project. This is a great way to ensure that your project is always in a working state.

## Setup runner image
You need to create a Docker image that contains all the tools you need to build and test your project.
Here is an example dockerfile that you can use as a starting point:
```dockerfile
FROM python:3.10-bullseye

COPY requirements.txt requirements.txt
COPY requirements.yml requirements.yml

RUN pip3 install -r requirements.txt
RUN ansible-galaxy install -r requirements.yml
```

## Setup GitLab CI/CD
To use GitLab CI/CD, you need to create a `.gitlab-ci.yml` file in the root of your project. This file contains the configuration for your CI/CD pipeline. You can find more information about the configuration file [here](https://docs.gitlab.com/ee/ci/yaml/).

Here is an example `.gitlab-ci.yml` file that you can use as a starting point:
```yaml
stages:
  - diff_infra
  - deploy_infra

workflow:
  rules:
    - if: $CI_COMMIT_BRANCH == $CI_DEFAULT_BRANCH

variables:
  ANSIBLE_CONFIG: ./ansible.cfg
  ANSIBLE_FORCE_COLOR: "true"

image:
  name: myrepo/ansible-runner:1.0.0

before_script:
    - pip3 install -r ansible/requirements.txt
    - ansible-galaxy install -r ansible/requirements.yml

diff infra:
  tags:
    - myrunner
  stage: diff_infra
  script:
    - echo $VAULT_PW > .vault-pass
    - ansible-playbook ../../ansible/pb-infra.yml --check --diff

deploy infra:
  tags:
    - myrunner
  stage: deploy_infra
  script:
    - echo $VAULT_PW > .vault-pass
    - ansible-playbook ../../ansible/pb-infra.yml
```
This will first run a `diff` on your infrastructure and then deploy it. You can add more stages to your pipeline to run more tests or build your application.