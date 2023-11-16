

Task2 Notes

Task 2 Instructions:
ON YOUR LOCAL MACHINE:

Modify flask-app/app.py to take in the database password as an environment variable
Fill in the Dockerfile for the flask app - the contents should be similar to the Dockerfile for task 1
Fill in the Dockerfile for the db, following the instructions provided
Create a Dockerfile for the custom NGINX image - this will be the same as the Dockerfile for the Task 1 reverse proxy
Build and push images for the three components

ON YOUR JENKINS VM:

Pull the nginx, app and db images from dockerhub
Create a new network called task2-net
Run containers from the three images - database first, then app, then nginx
deploy
 
Shell
docker run -d --name mysql --network task2-net -e MYSQL_ROOT_PASSWORD=<replace me> <dockerhub username>/task2-db
docker run -d --name flask-app --network task2-net -e MYSQL_ROOT_PASSWORD=<replace me> <dockerhub username>/task2-app
docker run -d --name nginx --network task2-net -p 80:80 <dockerhub username>/task2-nginx
