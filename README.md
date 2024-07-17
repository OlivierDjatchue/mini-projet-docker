## Firstname : Olivier

## Surname : Djatchue-Tchokothe

## For Eazytraining's 18th DevOps Bootcamp

## Period : march-april-may

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

1) Change directory and build the api container image :
```bash
cd ./simple_api
docker build . -t api.student_list.img
docker images
```
>![image](https://github.com/user-attachments/assets/2f6b6222-c8d0-42b3-9d64-5e76f31260d1)

3) Move back to the root dir of the project and run the backend api container with those arguments :

```bash
cd ..
docker run --rm -d --name=api.student_list -v ./simple_api/student_age.json:/data/student_age.json api.student_list.img
docker ps
```
>![image](https://github.com/user-attachments/assets/0d159e2c-f4e8-4f60-aada-8dd58ec8d55c)

3) Run the following command to see the running conainer

>![image](https://github.com/user-attachments/assets/41c00b03-5f7a-404c-99ed-189d967ead12)

4. This command returns a list of students and their ages
```bash
curl -u toto:python -X GET http://localhost:5000/pozos/api/v1.0/get_student_ages
```
>![image](https://github.com/user-attachments/assets/366d0509-3ecd-4980-ac24-c293fcf4d76d)
6. Stop the Docker container
>![image](https://github.com/user-attachments/assets/0c9f438d-cb83-431f-8011-6d00ddb56670)
7. Delete the container
>![image](https://github.com/user-attachments/assets/f7ef8309-3c3c-49fe-adbe-1ea95369c2c2))

