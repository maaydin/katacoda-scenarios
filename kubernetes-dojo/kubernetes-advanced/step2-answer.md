Update deployment strategy for nginx as Recreate

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
        image: nginx
  strategy:
    type: Recreate
</pre>

`kubectl apply -f webserver-dev.yml`{{execute}}.
`kubectl rollout status deployment webserver-dev`{{execute}}.
`kubectl get pods`{{execute}}.

Update deployment to nginx version 1.10.3 using --record option

`kubectl set image deployment webserver-dev nginx=nginx:1.11.5 --record`{{execute}}.
`kubectl rollout status deployment webserver-dev`{{execute}}.
`kubectl get pods`{{execute}}.

Update deployment strategy for nginx as RollingUpdate with max surge 20% and max unavailable 20%

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
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 20%
      maxUnavailable: 20%
</pre>

`kubectl apply -f webserver-dev.yml`{{execute}}.
`kubectl rollout status deployment webserver-dev`{{execute}}.
`kubectl get pods`{{execute}}.

Update deployment to nginx version 1.11.5 using --record option

`kubectl set image deployment webserver-dev nginx=nginx:1.11.5 --record`{{execute}}.
`kubectl rollout status deployment webserver-dev`{{execute}}.
`kubectl get pods`{{execute}}.

