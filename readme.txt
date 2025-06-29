1.After unzipping folder open your docker console

2.In the docker console navigate to this folder. For example on 
windows systems you need to use the command <cd 'absolute path'>, example -
	cd "D:\work\docker\GreenCityDocker"

3.Now use command <docker-compose build>. It will build all the images needed to run
GreenCityMvp.

4.After succesfull build use the command <docker-compose up>. This will start the
process of running all of the containers. Beware that database sql is embedded into
core-service, which means that user-service might crash during the first launch. 
If that is the case wait untill core-service create the database and then stop the
containers and use <docker-compose up> again. It will resolve this issue. To stop all
containers you need to press "ctrl + C" on the keyboard on Windows systems. You can
also stop them manually by writting <docker stop 'id of the container'>. To see all
of the containers running and their ids write "docker ps".

5.Now when user-service, core-service, ui-service, postgres_server and pgadmin are
running you can access them via browser.If you are using Docker Desktop make use of
this url - http://localhost:4205/ and proceed to STEP 6. In case you are using Docker ToolBox
you need to do "port forwarding". You can find information how to do this via this link -
https://www.jhipster.tech/tips/020_tip_using_docker_containers_as_localhost_on_mac_and_windows.html
Create 5 new entries
	
Name 		  Protocol   	Main-Ip     Main-port  Guest-IP  Guest-port
core-service	    TCP	       127.0.0.1	8085		    8085
pgadmin	            TCP        127.0.0.1	5052		    5052
postgres_server     TCP        127.0.0.1	2345		    2345
ui-service          TCP        127.0.0.1	4205		    4205
user-service        TCP        127.0.0.1	8065		    8065

Save this configuration and now you can access frontend by the same link http://localhost:4205/

6. After accessing frontend part by port 4205 you can login. To do this you need to
register a new user. Press 'register' button and enter credentials.

7.To enter as a new user you need to verify email. In order to do this we need to
change the user status and delete its email from the table "verify_emails".
To do this open a new docker console and write 
	"docker exec -it postgres_server bin/bash".

You will see root user. Then enter the command 
	"psql -U greencity". 

This will open psql for greencity role. The next command is 
	"\c greencity" 

which will select 'greencity' database. Now enter sql script:
	"delete from verify_emails where id=1;"

to make sure the entry was deleted you can write
	"select * from verify_emails;"

after this write the command 
	"select * from users;"

and find your user id. The last command is 
	"update users set user_status=2 Where id=<id of your user>;"

8.Now you can go back to login page and login as your user.


Additional info

Stop
1)"docker-compose stop" - stop all the containers that were run by "docker-compose up"

LAUNCH
1)"docker-compose build" - create images for latest app version
1)"docker-compose up" launch containers and aggregate the output of each container(repeat command if one or any containers shut down) - ctrl + c to exit
2)"docker-compose up -d" launch conrainers in background  - "docker-compose stop" stop containers

DELETE
1)"docker container rm -f $(docker container ls -aq)" remove all containers
2)"docker rmi -f $(docker images -a -q)" remove all images
3)"docker volume rm greencity-docker_postgres-data" remove postgress volume - beware, it will remove all database data

INFO
1)"docker ps -a" list all containers
2)"docker images" list images
3)"docker network ls" list networks
4)"docker volume ls" list volumes


Note!
google-creds.json, env_file, env_postgres - they all are secret files.

For more information read the docker and docker-compose documentation.