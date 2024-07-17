## Firstname : Olivier

## Surname : Djatchue-Tchokothe

## For Eazytraining's 18th DevOps Bootcamp

## Period : march-april-may

Our task is to deploy an application called "api", which displays a list of students and their ages. Our task is divided into 3 parts:

The first part consists of using docker commands to display the list of students.
The second part consists of displaying the list of students using docker-compose
The third part is to push our created image to the registry.
Work plan
We're going to set up the commands for the various containers

We're going to tag our image after running the api container that will allow us to see the list of students
We're going to set up a container using docker-compose.yml for displaying student information
We're going to create the registry using the docker-compose-registry so that we can host our application locally


# Part 1: Build the image and run the container

1) Change directory and build the api container image :
```bash
cd ./simple_api
docker build . -t api.student_list_img
docker images
```
>![image](https://github.com/user-attachments/assets/2f6b6222-c8d0-42b3-9d64-5e76f31260d1)

2) Move back to the root dir of the project and run the backend api container with those arguments :

```bash
cd ..
docker run --rm -d --name=api.student_list -v ./simple_api/student_age.json:/data/student_age.json api.student_list.img
docker ps
```
>![image](https://github.com/user-attachments/assets/0d159e2c-f4e8-4f60-aada-8dd58ec8d55c)

3) Run the following command to see the running conainer

>![image](https://github.com/user-attachments/assets/41c00b03-5f7a-404c-99ed-189d967ead12)

4) This command returns a list of students and their ages
```bash
curl -u toto:python -X GET http://localhost:5000/pozos/api/v1.0/get_student_ages
```
>![image](https://github.com/user-attachments/assets/366d0509-3ecd-4980-ac24-c293fcf4d76d)
5) Stop the Docker container
>![image](https://github.com/user-attachments/assets/0c9f438d-cb83-431f-8011-6d00ddb56670)
6) Delete the container
>![image](https://github.com/user-attachments/assets/f7ef8309-3c3c-49fe-adbe-1ea95369c2c2)
# Part 2: Infrastructure as code with docker compose
1) go to the previous folder and run the docker compose up command
   ```bash
   cd ..
   docker-compose up -d
   ```
> ![image](https://github.com/user-attachments/assets/814caf8f-511f-4e4b-a928-cb05657968a5)
2) Run the command docker ps to see the running containers
> ![image](https://github.com/user-attachments/assets/180a111d-5ef2-46ff-8e1a-a76698661298)
3) Open the port 80 of the docker labs
> ![image](https://github.com/user-attachments/assets/422fb3e0-f5a6-4521-9bf4-6a8288c014fb)
4) Trying to show the student list 
> ![image](https://github.com/user-attachments/assets/24dee22e-38f1-4e73-968d-076e3b9965e2)
5) The following error show that i habe to change to udpate the file index.php in order to fix the issue
```bash
vi /webapp/index.php
````
>![image](https://github.com/user-attachments/assets/e5894bbe-3129-44c7-8333-9cc56c658458)

6) Than click again on "List Student"
![image](https://github.com/user-attachments/assets/361b0749-950c-47dd-942d-0f6614586826)







