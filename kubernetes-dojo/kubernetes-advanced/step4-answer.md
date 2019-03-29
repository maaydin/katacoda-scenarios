Create an emptyDir volume and mount it to nginx pod

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
        volumeMounts:
          - name: html
            mountPath: /usr/share/nginx/html
            readOnly: true
      volumes:
      - name: html
        emptyDir: {}
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 20%
      maxUnavailable: 20%
</pre>

`kubectl apply -f webserver-dev.yml`{{execute}}.

Now nginx should return 403 forbidden as empty volume has no read permission to nginx user
https://[[HOST_SUBDOMAIN]]-30080-[[KATACODA_HOST]].environments.katacoda.com/

Update volume as a hostpath volume

- First create a directory and an index.html file in it.

`mkdir -p /tmp/hostmount`{{execute}}.
`echo "<h1>Hostmount</h1>" >> /tmp/hostmount/index.html`{{execute}}.

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
        volumeMounts:
          - name: html
            mountPath: /usr/share/nginx/html
            readOnly: true
      volumes:
      - name: html
        hostPath:
         path: "/tmp/hostmount"
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 20%
      maxUnavailable: 20%
</pre>

`kubectl apply -f webserver-dev.yml`{{execute}}.

Now nginx should serve the index.html at /tmp/hostmount
https://[[HOST_SUBDOMAIN]]-30080-[[KATACODA_HOST]].environments.katacoda.com/

- Delete your pod and recreate a new nginx and check if index.html is the same

`kubectl delete pod -l app=B2B`{{execute}}.

Create a persistent volume

<pre class="file" data-filename="pv.yml" data-target="replace">
apiVersion: v1
kind: PersistentVolume
metadata:
  name: test-volume
spec:
  capacity:
    storage: 1Gi
  accessModes:
  - ReadWriteOnce
  hostPath:
    path: /tmp/pv
</pre>

`kubectl apply -f pv.yml`{{execute}}.

Create a persistent volume claim with 1GB

<pre class="file" data-filename="pvc.yml" data-target="replace">
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: webserver-pv-claim
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
</pre>

`kubectl apply -f pvc.yml`{{execute}}.

Mount PVC to your nginx pod and create an index.html under /usr/share/nginx/html folder

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

`kubectl exec it <pod-id> -- bash`{{execute}}.

Now nginx should serve the index.html at /tmp/pv
https://[[HOST_SUBDOMAIN]]-30080-[[KATACODA_HOST]].environments.katacoda.com/

Delete your pod and recreate a new nginx and check if index.html is the same

`kubectl delete pod -l app=B2B`{{execute}}.