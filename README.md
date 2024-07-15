Firstname : Olivier

Surname : Djatchue-Tchokothe

For Eazytraining's 18th DevOps Bootcamp

Period : march-april-may

Period: march-april-may
Application
Our task is to deploy an application called "api", which displays a list of students and their ages. Our task is divided into 3 parts:

The first part consists of using docker commands to display the list of students.
The second part consists of displaying the list of students using docker-compose
The third part is to push our created image to the registry.
Work plan
We're going to set up the commands for the various containers

We're going to tag our image after running the api container that will allow us to see the list of students
We're going to set up a container using docker-compose.yml for displaying student information
We're going to create the registry using the docker-compose-registry so that we can host our application locally


Part 1

1. Change directory and build the api container image :
cd ./mini-projet-docker/simple_api
docker build . -t api.student_list.img
docker images
