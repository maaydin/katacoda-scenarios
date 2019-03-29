Deployment provides declarative updates for Pod/Pods.

Note: A Deployment’s rollout is triggered if and only if the Deployment’s pod template (that is, .spec.template) is changed, for example if the labels or container images of the template are updated. Other updates, such as scaling the Deployment, do not trigger a rollout.

## Katas

Using kubectl, find the required fields of a deployment object

Create nginx deployment with 1 replica

Create nginx deployment with 3 replicas

Delete your pod

Scale down from 3 to 1

Update deployment to nginx version 1.10.3 with 10 replicas and review the rollout status

Update deployment to nginx version 1.11.5 using --record option

Check the rollout history

Rollback the first revision
