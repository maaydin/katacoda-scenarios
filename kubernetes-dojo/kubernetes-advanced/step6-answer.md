Create a secret with different values 

<pre class="file" data-filename="secret.yml" data-target="replace">
apiVersion: v1
kind: Secret
metadata:
  name: secret
data:
  api_secret: a2xvaWFkb2pvCg==
  api_key: YzJSbVpHZGxjbmQzTkdSb1ozTm1OalF6Cg==
</pre>

`kubectl apply -f secret.yml`{{execute}}.

Mount env vars to pod using secret 

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
        env:
          - name: API_SECRET
            valueFrom:
              secretKeyRef:
                name: secret
                key: api_secret
          - name: API_KEY
            valueFrom:
              secretKeyRef:
                name: secret
                key: api_key
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

Mount secret as volume to the path "/etc/secrets" in the pod

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
        env:
          - name: API_SECRET
            valueFrom:
              secretKeyRef:
                name: secret
                key: api_secret
          - name: API_KEY
            valueFrom:
              secretKeyRef:
                name: secret
                key: api_key
        envFrom:
          - configMapRef:
              name: config
        volumeMounts:
          - name: html
            mountPath: /usr/share/nginx/html
            readOnly: true
          - name: config
            mountPath: /config
          - name: secret
            mountPath: "/etc/secrets"
            readOnly: true
      volumes:
      - name: html
        persistentVolumeClaim:
          claimName: webserver-pv-claim
      - name: config
        configMap:
          name: config
      - name: secret
        secret:
          secretName: secret
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 20%
      maxUnavailable: 20%
</pre>

`kubectl apply -f webserver-dev.yml`{{execute}}.
