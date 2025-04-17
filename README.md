# symfony_CICDCD

## Deploy localy
Install the dependances
```
composer install
```

Create the database and make the migrations
```
symfony console doctrine:database:create
symfony console doctrine:migration:migrate
```

Try locally
```
symfony serve
```

## Deploy with Docker

If needed deploy a myslq container
```
docker run --name symfony-mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=root mysql
```

Get the IP adress of mysql container
```
docker inspect symfony-mysql
```

Change the connection string in the .env line 27 with the IP adress of mysql container
Create database in mysql container and make the migration
```
symfony console doctrine:database:create
symfony console doctrine:migration:migrate
```

Build the image and deploy as container
```
docker build . -t symfony-cicdcd
docker run --name symfony_cicdcd_container -p 8080:80 symfony-cicdcd
```

## Deploy with Jenkins

If not already done start an instance of jenkins_master
```
docker run --name jenkins -p <choose_a_port>:8080 jenkins/jenkins
```

Then build and start an instance of a jenkins_agent
If your are on Windows, execute this command in Powershell or cmd
```
cd Jenkins-agent
docker build -t jenkins-agent-with-docker-and-composer .
docker run --init --name jenkins_agent_composer -v /var/run/docker.sock:/var/run/docker.sock jenkins-agent-with-docker-and-composer -url http://172.17.0.2:8080 c61658121264adf05f1571ecca888342ab74bfad780f6143b6012de8a15d2d19 symfony_CICDCD
```

Want to try the entire CICD on your own repository and registry ?