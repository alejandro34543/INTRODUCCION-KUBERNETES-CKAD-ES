# Pod Concepción
Los Pods son uno de los tipos de objetos más esenciales de Kubernetes. La mayoría de las características de orquestación de los Kubernetes se centran en la gestión de los Pods.

Los principios que se tratan para la gestión de los pods se aplican también a la gestión de otros tipos de objetos de Kubernetes.


## Creación

Crear un nuevo file yaml para contener la definición del Pod.
Usa el editor que quieras, aqui usamos vi :
```bash
vi my-pod.yml
```


```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
  labels:
    app: myapp
spec:
  containers:
  - name: myapp-container
    image: busybox
    command: ['sh', '-c', 'echo Hello Kubernetes! && sleep 3600']
```


Tambien podemos crear un Pod en command line sin usar vi :
```bash
$ kubectl run mypod --image=nginx --port=80 --restart=Never
```
Comprobamos con dry run para que no se applique directamente :
```bash
$ kubectl run mypod --image=nginx --port=80 --restart=Never --dry-run -o yaml
```

Y tambien mandar todo esto a un file si necesitamos manipular mas cosas con vi :
```bash
$ kubectl run mypod --image=nginx --port=80 --restart=Never --dry-run -o yaml > mypod.yml
```

## Documentacion
[Kubernetes Pods](https://kubernetes.io/docs/concepts/workloads/pods/pod/)

