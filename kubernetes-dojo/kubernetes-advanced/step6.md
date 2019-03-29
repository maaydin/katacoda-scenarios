Secrets enable container images to be created without bundling sensitive data.

## Katas

Create a secret with different values 
  
Content should be:   

<pre>
api_secret: a2xvaWFkb2pvCg==
api_key: YzJSbVpHZGxjbmQzTkdSb1ozTm1OalF6Cg==
</pre>

Mount env vars to pod using secret 

Mount secret as volume to the path "/etc/secrets" in the pod  

