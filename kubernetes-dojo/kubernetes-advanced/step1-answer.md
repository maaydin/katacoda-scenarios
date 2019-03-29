Using kubectl, find the required fields of a deployment object

`kubectl explain deployment`{{execute}}.

Create nginx deployment with 1 replica

<pre class="file" data-filename="webserver-dev.yml" data-target="replace">
apiVersion: apps/v1
kind: Deployment
metadata:
  name: webserver-dev
  labels:
    env: dev
    tier: frontend
    app: B2B
spec:
  selector:
    matchLabels:
      app: B2B
  template:
    metadata:
      labels:
        app: B2B
    spec:
      containers:
      - name: nginx
        image: nginx
</pre>

`kubectl apply -f webserver-dev.yml`{{execute}}.
`kubectl get deployments`{{execute}}.
`kubectl get pods`{{execute}}.

Create nginx deployment with 3 replicas

<pre class="file" data-filename="webserver-dev.yml" data-target="replace">
apiVersion: apps/v1
kind: Deployment
metadata:
  name: webserver-dev
  labels:
    env: dev
    tier: frontend
    app: B2B
spec:
  replicas: 3
  selector:
    matchLabels:
      app: B2B
  template:
    metadata:
      labels:
        app: B2B
    spec:
      containers:
      - name: nginx
        image: nginx
</pre>

`kubectl apply -f webserver-dev.yml`{{execute}}.
`kubectl get deployments`{{execute}}.
`kubectl get pods`{{execute}}.

Delete your pod

`kubectl delete pod -l app=B2B`{{execute}}.

Scale down from 3 to 1

`kubectl scale --replicas=1 deployment webserver-dev`{{execute}}.
`kubectl get pods`{{execute}}.

Update deployment to nginx version 1.10.3 with 10 replicas and review the rollout status

<pre class="file" data-filename="webserver-dev.yml" data-target="replace">
apiVersion: apps/v1
kind: Deployment
metadata:
  name: webserver-dev
  labels:
    env: dev
    tier: frontend
    app: B2B
spec:
  replicas: 10
  selector:
    matchLabels:
      app: B2B
  template:
    metadata:
      labels:
        app: B2B
    spec:
      containers:
      - name: nginx
        image: nginx:1.10.3
</pre>

`kubectl apply -f webserver-dev.yml`{{execute}}.
`kubectl rollout status deployment webserver-dev`{{execute}}.

Update deployment to nginx version 1.11.5 using --record option

`kubectl set image deployment webserver-dev nginx=nginx:1.11.5 --record`{{execute}}.

Check the rollout history

`kubectl rollout history deployment webserver-dev`{{execute}}.

Rollback the first revision

`kubectl rollout undo deployment webserver-dev --to-revision=1`{{execute}}.

