# Resource Requirements & Secrets



## Resource Requirements

Kubernetes es una herramienta poderosa para administrar y utilizar los recursos disponibles para ejecutar los contenedores.

Las solicitudes y los límites de los recursos proporcionan un gran control sobre cómo se asignarán los recursos.



Especifique las solicitudes de recursos y los límites de recursos en el contenedor de la siguiente manera:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-resource-pod
spec:
  containers:
  - name: myapp-container
    image: busybox
    command: ['sh', '-c', 'echo Hello Kubernetes! && sleep 3600']
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"
```



Tambien podemon crearlo en command line de la manera siguiente :
```bash
$ kubectl run my-pod --image=busybox --requests=memory="64Mi",cpu="250m" --limits=memory="128Mi",cpu="500m" --restart=Never -o yaml > my-pod.yml
```


## Secrets

Kubernetes Secrets te permite almacenar y gestionar información sensible, como contraseñas, tokens de OAuth y claves ssh.

Almacenar información confidencial en un Secreto es más seguro y flexible que ponerla literalmente en una definición de Pod o en una imagen de un contenedor.


```yaml
apiVersion: v1
kind: Secret
metadata:
  name: my-secret
stringData:
  myKey: myPassword
```



Una vez que se crea un secreto, pasa los datos sensibles a los contenedores como una environment variables:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-secret-pod
spec:
  containers:
  - name: myapp-container
    image: busybox
    command: ['sh', '-c', "echo Hello, Kubernetes! && sleep 3600"]
    env:
    - name: MY_PASSWORD
      valueFrom:
        secretKeyRef:
          name: my-secret
          key: myKey
```

Es una buena idea borrar el file yaml que contiene los datos sensibles después de que el objeto secreto haya sido creado en el cluster.

## Documentacion
[Kubernetes Resource Requirements](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/#resource-requests-and-limits-of-pod-and-container)

[Kubernetes Secrets](https://kubernetes.io/docs/concepts/configuration/secret/)

