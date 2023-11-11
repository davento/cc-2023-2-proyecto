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
Planteando un segundo escenario en el cual se quieren crear volúmenes con datasets distintos y hacer un entrenamiento distribuído y paralelizado para mejorar el tiempo de entrenamiento y experimentar con el modelo. Dicha tarea requiere la creación de múltiples volúmenes con datasets distintos, por lo cual se usa Kubernetes para realizar esta tarea y herramientas de monitoreo para corroborar la integridad de los resultados.

De forma así, se cubre el enfoque de Cloud y AI/ML propuesto para este proyecto.

## Resultados
[Por completar]

## Conclusiones
[Por completar].

## Bibliografía

M. Doctor Yuste. 2021. *"How to Create a Docker Image with Jupyter Notebook and Kotlin."* Towards Data Science. [https://towardsdatascience.com/how-to-create-a-docker-image-with-jupyter-notebook-kotlin-2e8bbf212f81](https://towardsdatascience.com/how-to-create-a-docker-image-with-jupyter-notebook-kotlin-2e8bbf212f81)

S. Okada. 2020. *"How to Run Jupyter Notebook on Docker"*. Medium. [https://medium.com/p/7c9748ed209f](https://medium.com/p/7c9748ed209f)