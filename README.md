# etherpad

create a deployment and service:
```bash
kubectl create deployment etherpad --image=etherpad/etherpad
kubectl expose deployment etherpad --port=9001 --name=etherpad
```

Ingress value for etherpad:
```bash
kubectl apply -f - << EOF
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: etherpad
  annotations:
    nginx.org/websocket-services: etherpad
    cert-manager.io/cluster-issuer: letsencrypt
spec:
  tls:
  - hosts:
      - etherpad.k8s.shubhamtatvamasi.com
    secretName: etherpad-tls
  rules:
  - host: etherpad.k8s.shubhamtatvamasi.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: etherpad
            port:
              number: 9001
EOF
```

delete everything:
```bash
kubectl delete deploy/etherpad svc/etherpad ing/etherpad
```
