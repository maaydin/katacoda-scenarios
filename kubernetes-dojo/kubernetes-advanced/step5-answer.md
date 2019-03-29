Create a configmap with different values

<pre class="file" data-filename="configmap.yml" data-target="replace">
apiVersion: v1
kind: ConfigMap
metadata:
  name: config
data:
  databasename: mydb
  database_uri: mydb://localhost:27017
  keys: |
      image.tag=latest
      userid:13
</pre>

`kubectl apply -f configmap.yml`{{execute}}.

Mount all env vars to pod using configmap 

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
        envFrom:
          - configMapRef:
              name: config
        volumeMounts:
          - name: html
            mountPath: /usr/share/nginx/html
            readOnly: true
      volumes:
      - name: html
        persistentVolumeClaim:
          claimName: webserver-pv-claim
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 20%
      maxUnavailable: 20%
</pre>

`kubectl apply -f webserver-dev.yml`{{execute}}.

Mount configmap as volume to the pod

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
        envFrom:
          - configMapRef:
              name: config
        volumeMounts:
          - name: html
            mountPath: /usr/share/nginx/html
            readOnly: true
          - name: config
            mountPath: /config
      volumes:
      - name: html
        persistentVolumeClaim:
          claimName: webserver-pv-claim
      - name: config
        configMap:
          name: config
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 20%
      maxUnavailable: 20%
</pre>

`kubectl apply -f webserver-dev.yml`{{execute}}.
