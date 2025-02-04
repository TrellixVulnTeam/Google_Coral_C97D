# Docker Container Set Up

Setps to follow to set up the docker container to be able to perform Transfer Learning on the Google Coral Dev Board. 


1) First install Docker on your desktop machine.

    Link: https://docs.docker.com/install/

2) Create a directory where you want to save the retrained model files. For example:

        DETECT_DIR=${HOME}/edgetpu/detection && mkdir -p $DETECT_DIR

3) Download our Dockerfile and build the image:

        cd $DETECT_DIR

        wget -O Dockerfile "http://storage.googleapis.com/cloud-iot-edge-pretrained-models/docker/obj_det_docker"

        sudo docker build - < Dockerfile --tag detect-tutorial

4) Start the Docker container using the new directory as a bind mount:

        docker run --name edgetpu-detect \
        --rm -it --privileged -p 6006:6006 \
        --mount type=bind,src=${DETECT_DIR},dst=/tensorflow/models/research/learn_pet \
        detect-tutorial

*If you find a problem in step number 4, you might not be able to access the docker engine, because you’re lacking permissions to access the unix socket to communicate with the engine.

If you are in a hurry adding `sudo` at the beggining of the command will solve the issue for now, but the best thing would be to run the following the comand:
        
        sudo usermod -a -G docker $USER

- For this you need to specify the `USER`:

        cut -d: -f1 /etc/passwd

- With that command we list all the users, then we just type the user we want to add to the group

        sudo usermod -a -G docker tomas_user

This will add the user to a Group and will let you run those type of commands without any problem.        

If this does not work (sometimes you might need to logout and login / restart terminal), there is an alternative solution:

Run the following command in order to avoid having to log in/out all the time:
     
        newgrp docker

After this, run again the add user command:

        sudo usermod -aG docker $USER

Where $USER would be the users to add to the docker group (root & tomruarol for example).*

After this, your terminal should now show your command prompt inside the Docker container.