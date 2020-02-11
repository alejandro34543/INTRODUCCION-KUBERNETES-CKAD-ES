# Conceptos Principales


[Conceptos Kubernetes](https://kubernetes.io/es/docs/concepts/#)

## Kubernetes master
El master Kubernetes es responsable de mantener el estado deseado de su grupo. Cuando interactúas con Kubernetes, por ejemplo, utilizando la interfaz de línea de comandos de kubectl, te comunicas con el maestro de los Kubernetes de tu clúster.

El "maestro" se refiere a un conjunto de procesos que gestionan el estado de la agrupación. Por regla general, todos los procesos se ejecutan en un solo Node del cúmulo. Este nodo también se conoce como el master. El master también puede ser replicado por disponibilidad y redundancia.
 



```bash
$ kubectl api-resources -o name
```

## Kubernetes Nodes
En un clúster de Kubernetes, los Nodes son las máquinas (máquinas virtuales, servidores físicos, etc) que ejecutan tus aplicaciones y flujos de trabajo en la nube. El master de Kubernetes controla cada nodo, por lo que en raras ocasiones interactuarás con los nodos directamente.


Para ver todos los nodes que tenemos y describirlos :
```bash
$ kubectl get nodes
$ kubectl get nodes nombre_node
$ kubectl describe nodes nombre_node
```

## Pod
Un Pod es la unidad básica de ejecución de una aplicación de Kubernetes, la unidad más pequeña y sencilla del modelo de objetos de Kubernetes que usted crea o despliega.
Un Pod representa los procesos que se ejecutan en su Cluster.

Un Pod encapsula el contenedor de una aplicación (o, en algunos casos, varios contenedores), los recursos de almacenamiento, una IP de red única y las opciones que rigen la forma en que deben ejecutarse los contenedores.

Un Pod representa una unidad de despliegue: una única instancia de una aplicación en Kubernetes, que puede consistir en un único contenedor o en un pequeño número de contenedores que están estrechamente acoplados y que comparten recursos.

## Kubectl
La herramienta de línea de comandos de Kubernetes, kubectl, le permite ejecutar comandos contra los clusters de Kubernetes.

Puede utilizar kubectl para desplegar aplicaciones, inspeccionar y gestionar los recursos del clúster y ver los registros. Para obtener una lista completa de las operaciones de kubectl.

[Saber mas sobre Kubectl](https://kubernetes.io/es/docs/tasks/tools/install-kubectl/)
