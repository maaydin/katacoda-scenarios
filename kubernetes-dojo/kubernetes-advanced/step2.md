Deployments are intended to replace Replication Controllers.  They provide the same replication functions (through Replica Sets) and also the ability to rollout changes and roll them back if necessary.

A Replication Controller is a structure that enables you to easily create multiple pods, then make sure that that number of pods always exists. If a pod does crash, the Replication Controller replaces it.

Deployments are recommended, since they are declarative, server side, and have additional features, such as rolling back to any previous revision even after the rolling update is done.

## Katas

Update deployment strategy for nginx as Recreate

Update deployment to nginx version 1.10.3 using --record option

Update deployment strategy for nginx as RollingUpdate with max surge 20% and max unavailable 20%

Update deployment to nginx version 1.11.5 using --record option
