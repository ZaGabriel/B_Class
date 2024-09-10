Connection :

192.168168.10(PC)	->	192.168.168.11(communication module)
192.168.168.12(Jetson)	->	192.168.168.13(communication module)

----------------------------------------------------------------------

Jetson :

# Run docker container
$ docker -d --rm --privileged -p 8080:22 --device /dev/video0:/dev/video0 jetson


PC :

# create access to X11
$ xhost +local:docker
# run docker container
$ docker run -it --rm --env="DISPLAY" --volume="/tmp/.X11-unix:/tmp/.X11-unix:rw" b_class
# Inside contianer ssh to Jetson
$ ssh -X root@192.168.168.12 -p8080
# Go to work directory
$ cd /app/Camera_Test
# Run code
$ python3 camera_test.py


