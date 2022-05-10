# gitlab_pipeline_examples

## trigger downstream project builds

https://docs.gitlab.com/ee/ci/yaml/#trigger

### Job1
```
variables:
  DOCKER_TAG: 1.0.1-release

stages:
  - test
  - deploy

test:
  stage: test
  script:
    - |
      echo $DOCKER_TAG

deploy:
  stage: deploy
  trigger: awiechert/job2
```

### Job 2
```
deploy:
  stage: deploy
  script:
    - echo $DOCKER_TAG
```

### Result

![upstream_pipeline](gitlab_trigger_downstream_pipeline.jpeg)


## trigger with manual click

### Job1
```
variables:
  DOCKER_TAG: 1.0.1-release

stages:
  - test
  - deploy

test:
  stage: test
  script:
    - |
      echo $DOCKER_TAG

deploy:
  stage: deploy
  trigger: awiechert/job2
  when: manual
```

![gitlab_manual1](gitlab_manual1.jpeg)
![gitlab_downstream_pipeline2](gitlab_downstream_pipeline2.jpeg)

## prefilled variables

### Job1
```
variables:
 K8S_CLUSTER_NAME:
   description: "Deploy to which kubernetes cluster? Valid values are -> dev-level-1 dev-level-2 prod01" 
   value: "dev-level-1" 
 DOCKER_TAG: 1.0.1-release

stages:
  - test
  - deploy

test:
  stage: test
  script:
    - |
      echo $DOCKER_TAG
      echo $K8S_CLUSTER_NAME

deploy:
  stage: deploy
  trigger: a1ae440/job2
  when: manual
```

###Job2
```
deploy:
  stage: deploy
  script:
    - | 
      echo $DOCKER_TAG
      echo $K8S_CLUSTER_NAME
```
![gitlab_prefilled_variables1](gitlab_prefilled_variables1.jpeg)
![gitlab_prefilled_variables1](gitlab_prefilled_variables2.jpeg)