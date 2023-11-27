# Proyecto Cloud Computing (CS3P02) 2023-2

> Enlace vídeo checkpoint: [https://drive.google.com/file/d/1PCWxBeAoZjR_s5sOvEMN42a5Ga8jdNtH/view?usp=sharing](https://drive.google.com/file/d/1PCWxBeAoZjR_s5sOvEMN42a5Ga8jdNtH/view?usp=sharing)
> 
> Enlace vídeo final: [https://drive.google.com/file/d/1aY5lJhZSSLDEMF_9uwURPxChMEC7E7jU/view?usp=sharing](https://drive.google.com/file/d/1aY5lJhZSSLDEMF_9uwURPxChMEC7E7jU/view?usp=sharing)

## Acerca del Proyecto

### Instrucción
> *Definir una aplicación que servirá de base para experimentar conceptos de cloud. La aplicación necesita ser adecuada para ejecutarse o adaptarse a un entorno de nube.*

### Descripción General
El proyecto a desarrollar es una implementación en Cloud de un [proyecto de clasificación de imágenes](https://github.com/davento/ImageClassificationCS429/tree/main) hecho para el curso de [*CS429/529 Machine Learning - Spring 2023* en la UNM](https://www.cs.unm.edu/~estrada/cs529.php). Dicho proyecto consiste de un identificador de plantas que recibe una imagen de una planta como input y retorna su clasificación como output.

El proyecto base se desarrolló en un Jupyter notebook. El alcance para este plantamiento original lo dejó con limitaciones en la forma en la que se carga su información y el entrenamiento del modelo que utilizan. Visto esto y la oportunidad que plantea el proyecto del curso *CS3P02 Cloud Computing 2023-2* en UTEC, el objetivo del proyecto actual es integrar el Identificador de Plantas con cloud computing, bajo el marco de *Cloud Serverless* y *Cloud & AI/ML*.

## Procedimiento

### Fase 0: Código Base
El código original fue desarrollado en el lenguaje Python. Se probó desarrollar el modelo con MobileNet-v2, Inception V3, ResNet 50, VGGNet-16 y Xception. La última arquitectura mostró los mejores resultados, por lo cual se decidió utilizarla.

El desarrollo se realizó principalmente en Kaggle para aprovechar la GPU que este servicio ofrecía para optimizarlo. Sin embargo, puesto que al ejecutarlo de manera local el entrenamiento era muy lento. Asimismo, los datos para entrenarla son muy pesados, por lo cual también se tuvo que [utilizar el servicio de Kaggle para cargarlos](https://www.kaggle.com/competitions/plant-seedlings-classification-cs429529).

### Fase 1: Integración con Containers
Considerando un escenario en el que se quiere compartir el código desarrollado y dejar que otras personas puedan ejecutarlo, se planteó en primera instancia llevar el código a un container. Para ello se utilizan los [Stacks de Jupyter Notebooks](https://jupyter-docker-stacks.readthedocs.io/en/latest/using/selecting.html), específicamente la imagen de `jupyter/tensorflow-notebook`.

Esto permite también almacenar los datos de prueba, el código y los resultados en el contenedor, lo cual va de la mano con el desarrollo Cloud Serverless.

### Fase 2: Integración con Kubernetes
Planteando un segundo escenario en el cual se quieren crear volúmenes con datasets distintos y hacer un entrenamiento distribuído y paralelizado para mejorar el tiempo de entrenamiento y experimentar con el modelo. Dicha tarea requiere la creación de múltiples volúmenes, por lo cual se usa Kubernetes para realizar esta tarea. Se hizo uso del dashboard de Kubernetes para corroborar que el deployment se haya realizado de manera correcta.

De forma así, se cubre el enfoque de Cloud y AI/ML propuesto para este proyecto.

## Prerequisitos
- [Docker](https://docs.docker.com/get-docker/)
- [Kubernetes (Minikube)](https://minikube.sigs.k8s.io/docs/start/)

## Ejecución
Primero pullear la información del repositorio de GitHub para tener los archivos.

```git clone https://github.com/davento/cc-2023-2-proyecto.git```

Una vez se tengan, para correr el contenedor en **Docker**, indicar el puerto y la ubicación de los datos a montar en el volumen
```docker run -p 8888:8888 -v .:/home/jovyan/work strobebug/plant-classifier:1.0```

Con ello ya debería poder tenerse acceso.

Para correrlo en **Kubernetes**, inicializar Minikube
```minikube start```

También cargar el dashboard para corroborar que todo esté bien
```minikube dashboard url```

Luego cargar los pods
```kubectl apply -f .\deployment.yaml```
> Nótese que se tuvo que crear un [secret](https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/    ) para poder descargar la imagen de Docker en Kubernetes. Además, reemplazar la dirección de la carpeta para el mount en el `deployment.yaml`.

Tras ello, inicializar el servicio
```minikube service plant-classifier```

Después, visualizar los pods, sea desde el dashboard o la terminal utilizando
```kubectl get pods --all-namespaces```

Copiar el nombre del pod que corresponda al servicio y correr
```kubectl logs pod/<nombre del pod>```
Por ejemplo,
```kubectl logs pod/plant-classifier-5c85b89567-fqpm2```
Con ello extraer el token que sale al final del url y copiarlo en la interfaz mostrada o seleccionar el link directamente.

Con ello se debe tener acceso a la interfaz mediante minikube.

## Resultados
Se pudo practicar con herramientas de desarrollo en la nube y experimentar con ellos con un aplicativo. Asimismo, se lograron solucionar las limitaciones indicadas en el proyecto original puesto que pudieron correr los archivos sin necesidad de usar una plataforma de un tercero para cargar los archivos y, también se logró correr el notebook de manera local y de manera rápida y efectiva.

Por dicho motivo se indica que los resultados fueron satisfactorios para los objetivos planteados para el proyecto.

## Bibliografía

M. Doctor Yuste. 2021. *"How to Create a Docker Image with Jupyter Notebook and Kotlin."* Towards Data Science. [https://towardsdatascience.com/how-to-create-a-docker-image-with-jupyter-notebook-kotlin-2e8bbf212f81](https://towardsdatascience.com/how-to-create-a-docker-image-with-jupyter-notebook-kotlin-2e8bbf212f81)

S. Okada. 2020. *"How to Run Jupyter Notebook on Docker"*. Medium. [https://medium.com/p/7c9748ed209f](https://medium.com/p/7c9748ed209f)

Docker. 2023. *Deployment and orchestration*. Docker Docs. [https://docs.docker.com/get-started/orchestration/](https://docs.docker.com/get-started/orchestration/)

Kubernetes. 2023. *Pull an Image from a Private Registry*. Kubernetes Documentation. [https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/](https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/#create-a-secret-by-providing-credentials-on-the-command-line)

Kubernetes. 2023. *Deploy and Access the Kubernetes Dashboard*. Kubernetes Documentation. [https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/)

Kubernetes. 2023. *Configure a Pod to Use a PersistentVolume for Storage*. Kubernetes Documentation. [https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage/](https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage/)

J. Chen. 2021. *[k8s] How to mount local directory (persistent volume) to Kubernetes pods of Docker Desktop for Mac?* Medium. [https://julien-chen.medium.com/k8s-how-to-mount-local-directory-persistent-volume-to-kubernetes-pods-of-docker-desktop-for-mac-b72f3ca6b0dd](https://julien-chen.medium.com/k8s-how-to-mount-local-directory-persistent-volume-to-kubernetes-pods-of-docker-desktop-for-mac-b72f3ca6b0dd)

## Anexos

[Imagen en Docker](https://hub.docker.com/repository/docker/strobebug/plant-classifier/general)