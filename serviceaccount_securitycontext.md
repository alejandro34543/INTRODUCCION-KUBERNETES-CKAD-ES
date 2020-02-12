# ServiceAccounts & SecurityContexts



## ServiceAccounts

Kubernetes permite que los contenedores que se ejecutan dentro del clúster interactúen con la API de Kubernetes. Esto abre la puerta a algunas formas poderosas de automatización.

Pero para garantizar que esto se haga de forma segura, es una buena idea utilizar cuentas de servicio especializadas con permisos restringidos para permitir que los contenedores accedan a la API. 

```bash
$ kubectl create serviceaccount my-serviceaccount
```

Cuando usted (un humano) accede al clúster (por ejemplo, utilizando kubectl), se autentica como una cuenta de usuario particular (en la actualidad, suele ser el administrador, a menos que el administrador del clúster haya personalizado el clúster).

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-serviceaccount-pod
spec:
  serviceAccountName: my-serviceaccount
  containers:
  - name: myapp-container
    image: busybox
    command: ['sh', '-c', "echo Hello, Kubernetes! && sleep 3600"]
```



Los procesos en contenedores dentro de las Pods también pueden contactar con el APISERVER. Cuando lo hacen, se autentican como una cuenta de servicio particular (por ejemplo, por defecto).




## SecurityContexts

En ocasiones, es necesario personalizar la forma en que los contenedores interactúan con los mecanismos de seguridad subyacentes presentes en los sistemas operativos de los nodos de Kubernetes.

El atributo securityContext en la especificación de un pod permite realizar estas personalizaciones.
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-securitycontext-pod
spec:
  securityContext:
    runAsUser: 2001
    fsGroup: 3001
  containers:
  - name: myapp-container
    image: busybox
    command: ['sh', '-c', "cat /message/message.txt && sleep 3600"]
    volumeMounts:
    - name: message-volume
      mountPath: /message
  volumes:
  - name: message-volume
    hostPath:
      path: /etc/message
```
Para especificar los ajustes de seguridad de un Pod, incluya el campo securityContext en la especificación del Pod.

El campo SecurityContext es un objeto PodSecurityContext. Los ajustes de seguridad que se especifican para un Pod se aplican a todos los contenedores del Pod.



## Documentacion
[Kubernetes ServiceAccounts](https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/)

[Kubernetes SecurityContexts](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/)

