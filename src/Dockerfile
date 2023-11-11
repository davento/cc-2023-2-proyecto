ARG BASE_CONTAINER=jupyter/tensorflow-notebook
FROM $BASE_CONTAINER
USER root
COPY /train /test
RUN pip install numpy cv2 glob tqdm random matplotlib plotly sklearn
USER $NB_UID