# symfony_CICDCD

Install the dependances
```
composer install
```

Try locally
```
symfony serve
```

Build the image and deploy as container
```
docker build . -t symfony-cicdcd
docker run --name symfony_cicdcd_container -p 8080:80 symfony-cicdcd
```

CICDCD in Docker environment

Require :
- Docker Engine instal

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