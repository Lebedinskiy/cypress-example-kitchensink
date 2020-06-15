In this example, we will launch concourse using docker compose file and create CI/CD.
First of all you need to run docker container with concourse, you can take official docker-compose file on https://github.com/concourse/concourse-docker/blob/master/docker-compose.yml 
or you can use my file.


command:  docker-compose up -d


NOTE!  If you use external instance you will need to change your docker-compose file from CONCOURSE_EXTERNAL_URL: http://localhost:8080 
to CONCOURSE_EXTERNAL_URL: http://your-ip-address:8080.


Connecting to the Server

Once the server is started, the web interface of the Concourse CI can be accessed by going to http://servers_public_IP:8080 in any browser.
Log in using the username 'test' and password 'test'.

To install the Concourse CLI (fly) on your system, click on the Linux logo to download, and run the following commands…

$ cd ~/Downloads/
$ install fly /usr/local/bin

$ which fly
/usr/local/bin/fly

$ fly -v
6.2.0


Last step is to login. Using the fly login command will help accomplish this.

$ fly login -t hello -u test -p test -c http://servers_public_IP:8080
logging in to team 'main'

target saved

Note: The -t flag is a target alias and is required for almost every command.
This alias comes in very handy when you’re targeting multiple concourse installations.

Setting up a Pipeline

Grabbing a bare bones Concourse pipeline is a good way to start kicking the tires. Luckily, there are lots of examples to choose from in the Concourse repos.

In our case we will use my pipeline which located https://github.com/Lebedinskiy/cypress-example-kitchensink/blob/master/ci/pipeline.yml

What this pipeline will do:

1. Get github repo
2. Create Dockerfike
3. Build docker image
4. Push docker image to aws ecr
5. Get our image and test him
6. Put to our deploy repo on ecr

We need to set up aws ecs and connect with our deploy repo.



Result our pipeline you can see on the picture named 'done.png'