# Observación

Kubernetes suelen ser capaces de detectar problemas con los contenedores y responder adecuadamente sin necesidad de una configuración especializada.

Pero a veces necesitamos un control adicional sobre la forma en que los Kubernetes determinan el estado de los contenedores.

Los (probes) sondas de Kubernetes proporcionan la capacidad de personalizar la forma en que Kubernetes detecta el estado de los contenedores, permitiéndonos construir mecanismos más sofisticados para gestionar la salud de los contenedores.

## Liveness and Readiness Probes :

### LivenessProbe

Indica si el contenedor está funcionando. Si la liveness probe falla,  kubelet mata el Contenedor, y el Contenedor se somete a su política de reinicio.
Si un Contenedor no proporciona una liveness probe, el estado por defecto es Success.
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-liveness-pod
spec:
  containers:
  - name: myapp-container
    image: busybox
    command: ['sh', '-c', "echo Hello, Kubernetes! && sleep 3600"]
    livenessProbe:
      exec:
        command:
        - echo
        - testing
      initialDelaySeconds: 5
      periodSeconds: 5
```
Los Contenedores del Pod pueden leer y escribir todos los mismos archivos en el volumen emptyDir, aunque ese volumen puede montarse en la misma o en diferentes rutas en cada Contenedor.

### ReadinessProbe

kubelet utiliza readiness probes para saber cuándo un contenedor está listo para empezar a aceptar el tráfico. Un Pod se considera listo cuando todos sus contenedores están listos.

Uno de los usos de esta señal es controlar qué Pods se utilizan como backends de Servicios. Cuando un Pod no está listo, se retira de los loadbalancers de los Servicios.




```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-readiness-pod
spec:
  containers:
  - name: myapp-container
    image: nginx
    readinessProbe:
      httpGet:
        path: /
        port: 80
      initialDelaySeconds: 5
      periodSeconds: 5
```

Indica si el contenedor está listo para atender las requests. Si la readiness probe falla, los endpoints  elimina la address IP del Pod de los endpoints de todos los Servicios que coinciden con el Pod.

El estado de preparación predeterminado antes de la demora inicial es Failure.

Si un Contenedor no proporciona una readiness probe, el estado predeterminado es Success.





## Documentacion
[Kubernetes Probes](https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/#container-probes)


[Kubernetes Liveness and Readiness Probes ](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/)

