Application I had to deploy an application called "student_list", which is very simple and allows POZOS to display the list of some students with their age.

The student_list application has two modules:

The first module is a REST API (with basic authentication required) that send the desire list of students based on JSON file The second module is a web application written in HTML + PHP that allow end users to get a list of students The need My work was to build a :

create a container for each module make them interact with each other provide a private registry My plan First, I will introduce you to the six files of this project and their role.

Then I'll show you how I built and tested the architecture to justify my choices.

The third and final part will be about the deployment process I propose for this application.

The role of files In my deployment you will find three main files: a dockerfile, a docker-compose.yml and a docker-compose-registry.yml.

docker-compose.yml: to start the application (API and web app) docker-compose.registry.yml: to start the local registry and its frontend simple_api/student_age.py: contains the API source code in Python simple_api/Dockerfile: to build the API image with the source code in it simple_api/student_age.json: contains the student name with age in JSON format index.php: PHP page where the end user will connect to interact with the service to list students by age. Building and testing Given that you have just cloned this repository, you need to follow these steps to get the 'student_list' application ready:

Change the directory and build the api container image : cd ./mini-projet-docker/simple_api docker build . -t api_image


screenshot
docker images 1-docker images

Create a bridge-type network for the two containers to be able to contact each other using their names thanks to dns functions : docker network create student_list.network --driver=bridge docker network ls 2-docker network ls

Go back to the root directory of the project and run the backend api container with these arguments : cd .. docker run --rm -d --name=api.student_list --network=student_list.network -v ./simple_api/:/data/ api.student_list.img docker ps 3-docker ps

As you can see, the api backend container is listening on port 5000. This internal port can be reached by another container on the same network, so I decided not to expose it.

I also had to mount the local directory ./simple_api/ in the internal container directory /data/ so that the api could use the student_age.json list

4-./simple_api/:/data/

Update the index.php file: You will need to update the following line before running the website container to make the api_ip_or_name and port match your deployment $url = 'http://<api_ip_or_name:port>/pozos/api/v1.0/get_student_ages';

Thanks to the DNS capabilities of our bridge-type network, we can easily use the name of the API container with the port we just saw to customise our website

sed -i s<api_ip_or_name:port>\api.student_list:5000\g ./website/index.php 5-api.student_list:5000

Run the frontend webapp container : username and password are provided in the source .simple_api/student_age.py

6-id/passwd

docker run --rm -d --name=webapp.student_list -p 80:80 --network=student_list.network -v ./website/:/var/www/html -e USERNAME=toto -e PASSWORD=python php:apache docker ps 7-docker ps

Testing the api from the frontend : 

6a) Using the command line:

The next command will ask the frontend container to request the backend api and show you the output. The goal is to test both if the api works and if the frontend can get the student list from it
docker exec webapp.student_list curl -u toto:python -X GET http://api.student_list:5000/pozos/api/v1.0/get_student_ages 8-docker exec

6b) Using a web browser IP:80 :

If you're running the application on a remote server or virtual machine (e.g. provided by eazytraining's vagrant file), find your IP address by typing hostname -I 9-hostname -I

If you are using PlayWithDocker, simply open port 80 in the GUI. If not, type localhost:80.

10-Check the web page

Clean up the workspace: thanks to the --rm argument we used to start our containers, they will be removed when they stop. Remove the network we created earlier.

docker stop api.student_list docker stop webapp.student_list docker network rm student_list.network docker network ls docker ps 11-clean-up

Deployment With the tests passed, we can now 'composerise' our infrastructure by putting the docker run parameters in infrastructure as code format in a docker-compose.yml file.

Run the application (api + webapp): Since we've already created the application image, all we need to do now is run :

docker-compose up -d Docker-compose allows you to choose which container to start first. The api container will be the first, as I specified that the webapp depends_on: it.

12-depending on

And the application works:

13-check application

Creating a registry and its frontend I used registry:2 image for the registry and joxit/docker-registry-ui:static for its frontend gui and passed some environment variables:

14-gui registry env var

For example, we'll be able to delete images from the registry via the gui.

docker-compose -f docker-compose.registry.yml up -d 15-check gui reg

Put an image on the registry and test the gui You need to rename it first (:latest is optional):

Tag the image docker image tag api_image:v1 localhost:5001/pozos/api_image:v1 Push the image to the registry docker push localhost:5001/pozos/api.student_list.img:latest



