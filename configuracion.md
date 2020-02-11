# Configuracion
Esta página proporciona una serie de ejemplos de uso que demuestran cómo crear ConfigMaps y configurar Pods utilizando los datos almacenados en ConfigMaps.




## ConfigMap

La gestión de los datos de configuración es uno de los desafíos que plantea la construcción y el mantenimiento de infraestructuras de aplicaciones complejas.

Por suerte, Kubernetes ofrece una funcionalidad que ayuda a mantener las configuraciones de las aplicaciones en forma de ConfigMaps. 

Veremos qué son los ConfigMaps, cómo crearlos, algunas de las formas en que los datos de ConfigMap pueden pasarse a los contenedores que se ejecutan en los Pods de Kubernetes.


Creacion de una configmap rapidamente con los valores siguientes:
```bash
$ kubectl create cm my-config-map --from-literal=myKey=myValue
```

Ejemplo de configmap :
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
   name: my-config-map
data:
   myKey: myValue
   anotherKey: anotherValue
```


Tambien podemon crear una configmap a partir de un file :
```bash
$ kubectl create cm my-config-map --from-file=/etc/ejemplo/data.conf
```


## Configmap en el Pod
Pasar los datos de ConfigMap a un contenedor como environment variable se ve así :
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-configmap-pod
spec:
  containers:
  - name: myapp-container
    image: busybox
    command: ['sh', '-c', "echo $(MY_VAR) && sleep 3600"]
    env:
    - name: MY_VAR
      valueFrom:
        configMapKeyRef:
          name: my-config-map
          key: myKey
```



También es posible pasar los datos de ConfigMap a los contenedores, en forma de archivo usando un volumen montado, así :
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-configmap-volume-pod
spec:
  containers:
  - name: myapp-container
    image: busybox
    command: ['sh', '-c', "echo $(cat /etc/config/myKey) && sleep 3600"]
    volumeMounts:
      - name: config-volume
        mountPath: /etc/config
  volumes:
    - name: config-volume
      configMap:
        name: my-config-map
```

## Documentacion
[Kubernetes Configmap](https://kubernetes.io/fr/docs/home/)

