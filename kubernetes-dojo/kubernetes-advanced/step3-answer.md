Using kubectl, find the required fields of a service object

`kubectl explain service`{{execute}}.

Expose B2B to cluster
<pre class="file" data-filename="service.yml" data-target="replace">
apiVersion: v1
kind: Service
metadata:
  name: webserver-dev
  labels:
    env: dev
    tier: frontend
    app: B2B
spec:
  ports:
  - name: nginx-port
    port: 80
    targetPort: 80
    protocol: TCP
  selector:
    app: B2B
</pre>

`kubectl apply -f service.yml`{{execute}}.

Expose B2B to internet by opening a node port

<pre class="file" data-filename="loadbalancer.yml" data-target="replace">
apiVersion: v1
kind: Service
metadata:
  name: webserver-dev-loadbalancer
  labels:
    env: dev
    tier: frontend
    app: B2B
spec:
  ports:
  - name: nginx-port
    port: 80
    targetPort: 80
    protocol: TCP
    nodePort: 30080
  selector:
    app: B2B
  type: LoadBalancer
</pre>

`kubectl apply -f loadbalancer.yml`{{execute}}.

https://[[HOST_SUBDOMAIN]]-30080-[[KATACODA_HOST]].environments.katacoda.com/

Expose B2B to internet by creating an ingress

- First enable ingress on minikube.

`minikube addons enable ingress`{{execute}}.

<pre class="file" data-filename="ingress.yml" data-target="replace">
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: webserver-dev
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
 rules:
 - host: ingress.demo
   http:
     paths:
     - path: /*
       backend:
         serviceName: webserver-dev
         servicePort: 80
</pre>

`kubectl apply -f ingress.yml`{{execute}}.
`echo "127.0.0.1       ingress.demo" >> /etc/hosts`{{execute}}.
`curl -v ingress.demo`{{execute}}.

Check Endpoints of your service

`kubectl get services`{{execute}}.