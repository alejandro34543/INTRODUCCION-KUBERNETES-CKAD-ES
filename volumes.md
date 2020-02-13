# Volumes

Los files en disco en un contenedor son efímeros, lo que plantea problemas para las aplicaciones no triviales cuando se ejecutan en contenedores.

Primero, cuando un Contenedor se cae, kubelet lo reinicia, pero los archivos se pierden y el contenedor comienza con un estado limpio. En segundo lugar, cuando los contenedores se ejecutan juntos en un Pod, a menudo es necesario compartir archivos entre ellos.

La abstraction del volumen de Kubernetes resuelve ambos problemas.

El almacenamiento de los contenedores está diseñado para ser tan temporal como los propios contenedores.

Sin embargo, a veces necesitamos un almacenamiento que sea capaz de sobrevivir más allá de la corta vida de un contenedor. 

Los volumes de Kubernetes nos permiten montar el almacenamiento en un contenedor que no está en el propio contenedor.

## EmptyDir :

Un volumen emptyDir se crea primero cuando un Pod se asigna a un Node, y existe mientras ese Pod esté funcionando en ese Node.

Como su nombre indica, inicialmente está vacío. 

Este Pod monta un simple volumen emptyDir, my-volume, en el contenedor en el camino //tmp/storage :
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: volume-pod
spec:
  containers:
  - image: busybox
    name: busybox
    command: ["/bin/sh", "-c", "while true; do sleep 3600; done"]
    volumeMounts:
    - mountPath: /tmp/storage
      name: my-volume
  volumes:
  - name: my-volume
    emptyDir: {}
```
Los Contenedores del Pod pueden leer y escribir todos los mismos archivos en el volumen emptyDir, aunque ese volumen puede montarse en la misma o en diferentes rutas en cada Contenedor.

Cuando se elimina un Pod de un Node por cualquier motivo, los datos del emptyDir se eliminan para siempre.





## PersistentVolumes (PV)

Los PersistentVolumes  (PV) y los PersistentVolumeClaims (PVC) ofrecen una forma de consumir fácilmente los recursos de almacenamiento, especialmente en el contexto de un entorno de producción complejo que utiliza múltiples soluciones de almacenamiento. 


```yaml
kind: PersistentVolume
apiVersion: v1
metadata:
  name: my-pv
spec:
  storageClassName: local-storage
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/data"
```



Une vez creado podemos consultar su status con la siguiente commanda :
```bash
$ kubectl get pv
```

## PersistentVolumeClaims (PVC)
El principio es separar la declaración de un volumen persistente de su Pod.

Primero declaramos los PV con un proceso específico, luego adjuntamos el Pod a un volumen disponible con el PVC.



```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  storageClassName: local-storage
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 512Mi
```


Igualmente une vez creado ver su status y verificar si esta asociado con el PV creado antes "bound" :
```bash
$ kubectl get pvc
```

### Phase
**Un volumen estará en una de las siguientes fases :**

**Available** - un recurso libre que aún no está atado a un PVC

**Bound** - el volumen está atado a un PVC

**Released** - el PVC ha sido borrado, pero la resource aún no ha sido reclamado por el cluster.

**Failed** - el volumen ha fallado su recuperación automática.

La CLI mostrará el nombre del PVC unido al PV.



## Documentacion
[Kubernetes Volumes](https://kubernetes.io/docs/concepts/storage/volumes/#)

[Kubernetes PersistentVolumes (PV)](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)

[Kubernetes PersistentVolumeClaims (PVC)](https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage/)

