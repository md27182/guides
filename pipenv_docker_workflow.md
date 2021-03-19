## Using pipenv with docker
### Update Pipfile.lock
First clear out the .venv folder and launch a new container in bash mode
```
sudo docker run --gpus all -it --rm -v /home/lightbox/Desktop/lbx-nerf:/tmp -w /tmp train-nerf:latest bash
```
Then, from inside the container, run
```
pipenv install
```
Close the docker container. 

Note: it's important to update the lock file from inside the docker environment to avoid, for example, building dependencies based on your local python version instead of the one in the docker environment. The reason we couldn't update the lock file during the building of the image is that the updated lock file wouldn't be saved.

### Build image using new Pipfile.lock
Navigate to the lbx-nerf folder and run
```
sudo docker build --tag train-nerf .
```
Note: the Dockerfile should look like this
```
FROM tensorflow/tensorflow:2.4.1-gpu
RUN pip install pipenv
COPY Pipfile Pipfile
COPY Pipfile.lock Pipfile.lock
RUN pipenv install --system
CMD python3 run_nerf.py --config config_train.txt
```

### Run the code
```
sudo docker run --gpus all -it --rm -v /home/lightbox/Desktop/lbx-nerf:/tmp -w /tmp train-nerf:latest python run_nerf.py --config config_train.txt
```
