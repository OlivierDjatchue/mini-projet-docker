## Firstname : Olivier

## Surname : Djatchue-Tchokothe

## For Eazytraining's 18th DevOps Bootcamp

## Period : march-april-may

Our task is to deploy an application called "api" that displays a list of students and their ages. Our task is divided into 3 parts:

The first part is to use docker commands to display the list of students.
The second part is to display the list of students using docker-compose.
The third part is to push our created image to the registry.
Work plan
We'll set up the commands for the different containers

We're going to tag our image after running the API container that will allow us to see the list of students.
We'll set up a container using docker-compose.yml to display student information.
We'll create the registry using docker-compose-registry so that we can host our application locally.

# Part 1:  Building the image and running the container

1) Change directory and build the api container image :
```bash
cd ./simple_api
docker build . -t api.student_list_img
docker images
```
>![image](https://github.com/user-attachments/assets/2f6b6222-c8d0-42b3-9d64-5e76f31260d1)

2) Go back to the root directory of the project and run the backend api container with these arguments

```bash
cd ..
docker run --rm -d --name=api.student_list -v ./simple_api/student_age.json:/data/student_age.json api.student_list.img
docker ps
```
>![image](https://github.com/user-attachments/assets/0d159e2c-f4e8-4f60-aada-8dd58ec8d55c)

3) Run the following command to see the running container

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
1) Go to the previous folder and run the docker compose up command
   ```bash
   cd ..
   docker-compose up -d
   ```
> ![image](https://github.com/user-attachments/assets/814caf8f-511f-4e4b-a928-cb05657968a5)
2) Run the command docker ps to see the running containers
> ![image](https://github.com/user-attachments/assets/180a111d-5ef2-46ff-8e1a-a76698661298)
3) Open the port 80 of the docker labs
> ![image](https://github.com/user-attachments/assets/422fb3e0-f5a6-4521-9bf4-6a8288c014fb)
4) Try to show the list of students 
> ![image](https://github.com/user-attachments/assets/24dee22e-38f1-4e73-968d-076e3b9965e2)
5) The previous errors show that I need to change the index.php file to fix the problem.
```bash
vi /webapp/index.php
````
>![image](https://github.com/user-attachments/assets/e5894bbe-3129-44c7-8333-9cc56c658458)

6) Then click again on "List Student"
![image](https://github.com/user-attachments/assets/361b0749-950c-47dd-942d-0f6614586826)
# Part 3: Pushing the image to a local registry
1) First of all tag the image
```bash
docker tag api.student_list_img:latest localhost:5000/api.student_list_img:latest
```
>![image](https://github.com/user-attachments/assets/be8826f2-cb33-4646-901b-c3211dcaa8cd)
2)Running the Registry and the Regestry ui
```bash
docker-compose -f docker-compose-registry.yml up -d
```
Then Open port 82 of the Docker Lab environment
>![image](https://github.com/user-attachments/assets/acebb539-7591-43de-b926-758029056c46)
3) Pushing the image to the local registry
```bash
docker push localhost:5000/api.student_list_img:latest
````
by reoading the page we should see the uploaded image
>![image](https://github.com/user-attachments/assets/16bfb733-a44e-455e-a2ce-8b820d2ff243)










