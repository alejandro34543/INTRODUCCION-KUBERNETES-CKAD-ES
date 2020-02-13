# Services 

Los Deployments facilitan la creación de un conjunto de réplicas de Pods que pueden ser scaled, updated y replaced dinámicamente.Sin embargo, proporcionar acceso a la red a esos pods para otros componentes es difícil.

Los servicios proporcionan una capa de abstracción que resuelve este problema. Los clientes pueden simplemente acceder al servicio, que dinámicamente proxy el tráfico al conjunto actual de réplicas. 


Creation de un deployment :
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.7.9
        ports:
        - containerPort: 80
```
Exponer las réplicas de los Pods del deployment con un servicio :
```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  type: ClusterIP
  selector:
    app: nginx
  ports:
  - protocol: TCP
    port: 8080
    targetPort: 80
```

Puedes obtener más información sobre el servicio con estos comandas :
```sh
$ kubectl get svc
$ kubectl get endpoints my-service
```

# NetworkPolicies

Desde el punto de vista de la seguridad, suele ser una buena idea imponer restricciones a nivel de la red a cualquier comunicación entre diferentes partes de su infraestructura.

Las NetworkPolicies le permiten restringir y controlar el tráfico de red que va y viene de sus Pods. 



Una NetworkPolicy que restrinja todo el acceso al Pod, excepto a los Pods que tienen el allow-access: "true" label :
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: my-network-policy
spec:
  podSelector:
    matchLabels:
      app: secure-app
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          allow-access: "true"
    ports:
    - protocol: TCP
      port: 80
  egress:
  - to:
    - podSelector:
        matchLabels:
          allow-access: "true"
    ports:
    - protocol: TCP
      port: 80
```
Los recursos de la NetworkPolicy utilizan etiquetas para seleccionar los Pods y definen reglas que especifican qué tráfico se permite a los Pods seleccionados :

Obtenga información sobre las NetworkPolicy en el cluster :
```bash
kubectl get networkpolicies
kubectl describe networkpolicy my-network-policy
```

Por defecto, los Pods no están aislados; aceptan tráfico de cualquier fuente.
Los pods se aíslan al tener una NetworkPolicy que los selecciona.

Una vez que hay una NetworkPolicies en un namespace  seleccionando un pod particular, ese pod rechazará cualquier conexión que no esté permitida por cualquier NetworkPolicies. 

(Otros pods en el espacio de nombres que no son seleccionados por NetworkPolicy continuarán aceptando todo el tráfico).





## Documentacion
[Kubernetes Service to Expose](https://kubernetes.io/docs/tutorials/kubernetes-basics/expose/expose-intro/)

[Kubernetes NetworkPolicies](https://kubernetes.io/docs/concepts/services-networking/network-policies/)


[Calico Doc](https://docs.projectcalico.org/v2.0/getting-started/kubernetes/)

