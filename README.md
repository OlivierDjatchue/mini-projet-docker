
Application
I had to deploy an application named "student_list", which is very basic and enables POZOS to show the list of some students with their age.

student_list application has two modules:

The first module is a REST API (with basic authentication needed) who send the desire list of the student based on JSON file
The second module is a web app written in HTML + PHP who enable end-user to get a list of students
The need
My work was to :

build one container for each module
make them interact with each other
provide a private registry
My plan
First, let me introduce you the six files of this project and their role

Then, I'll show you how I built and tested the architecture to justify my choices

Third and last part will be about to provide the deployment process I suggest for this application.

The files' role
In my delivery you can find three main files : a Dockerfile, a docker-compose.yml and a docker-compose-registry.yml

docker-compose.yml: to launch the application (API and web app)
docker-compose.registry.yml: to launch the local registry and its frontend
simple_api/student_age.py: contains the source code of the API in python
simple_api/Dockerfile: to build the API image with the source code in it
simple_api/student_age.json: contains student name with age on JSON format
index.php: PHP  page where end-user will be connected to interact with the service to list students with their age.
Build and test
Considering you just have cloned this repository, you have to follow those steps to get the 'student_list' application ready :

Change directory and build the api container image :
cd ./mini-projet-docker/simple_api
docker build . -t api_image
# screeshot
docker images
1-docker images

Create a bridge-type network for the two containers to be able to contact each other by their names thanks to dns functions :
docker network create student_list.network --driver=bridge
docker network ls
2-docker network ls

Move back to the root dir of the project and run the backend api container with those arguments :
cd ..
docker run --rm -d --name=api.student_list --network=student_list.network -v ./simple_api/:/data/ api.student_list.img
docker ps
3-docker ps

As you can see, the api backend container is listening to the 5000 port. This internal port can be reached by another container from the same network so I chose not to expose it.

I also had to mount the ./simple_api/ local directory in the /data/ internal container directory so the api can use the student_age.json list

4-./simple_api/:/data/

Update the index.php file :
You need to update the following line before running the website container to make api_ip_or_name and port fit your deployment  $url = 'http://<api_ip_or_name:port>/pozos/api/v1.0/get_student_ages';

Thanks to our bridge-type network's dns functions, we can easyly use the api container name with the port we saw just before to adapt our website

sed -i s\<api_ip_or_name:port>\api.student_list:5000\g ./website/index.php
5-api.student_list:5000

Run the frontend webapp container :
Username and password are provided in the source code .simple_api/student_age.py

6-id/passwd

docker run --rm -d --name=webapp.student_list -p 80:80 --network=student_list.network -v ./website/:/var/www/html -e USERNAME=toto -e PASSWORD=python php:apache
docker ps
7-docker ps

Test the api through the frontend :
6a) Using command line :

The next command will ask the frontend container to request the backend api and show you the output back. The goal is to test both if the api works and if frontend can get the student list from it.

docker exec webapp.student_list curl -u toto:python -X GET http://api.student_list:5000/pozos/api/v1.0/get_student_ages
8-docker exec

6b) Using a web browser IP:80 :

If you're running the app into a remote server or a virtual machine (e.g provisionned by eazytraining's vagrant file), please find your ip address typing hostname -I
9-hostname -I

If you are working on PlayWithDocker, just open the 80 port on the gui
If not, type localhost:80
Click the button

10-check webpage

Clean the workspace :
Thanks to the --rm argument we used while starting our containers, they will be removed as they stop. Remove the network previously created.

docker stop api.student_list
docker stop webapp.student_list
docker network rm student_list.network
docker network ls
docker ps
11-clean-up

Deployment
As the tests passed we can now 'composerize' our infrastructure by putting the docker run parameters in infrastructure as code format into a docker-compose.yml file.

Run the application (api + webapp) :
As we've already created the application image, now you just have to run :

docker-compose up -d
Docker-compose permits to chose which container must start first. The api container will be first as I specified that the webapp depends_on: it.

12-depends on

And the application works :

13-check app

Create a registry and its frontend
I used registry:2 image for the registry, and joxit/docker-registry-ui:static for its frontend gui and passed some environment variables :

14-gui registry env var

E.g we'll be able to delete images from the registry via the gui.

docker-compose -f docker-compose.registry.yml up -d
15-check gui reg

Push an image on the registry and test the gui
You have to rename it before (:latest is optional) :

Tag The image
docker image tag api_image:v1 localhost:5001/pozos/api_image:v1
Push the imahe to registry
docker push localhost:5001/pozos/api.student_list.img:latest

