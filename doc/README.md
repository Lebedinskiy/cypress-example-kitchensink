In this example, we will launch concourse using docker compose file and create CI/CD.
First of all you need to run docker container with concourse, you can take official docker-compose file on https://github.com/concourse/concourse-docker/blob/master/docker-compose.yml or you can use my file located in doc folder.

After download launch your docker compose file

$  docker-compose up -d


NOTE!  If you use external instance you will need to change your docker-compose file from CONCOURSE_EXTERNAL_URL: http://localhost:8080
to CONCOURSE_EXTERNAL_URL: http://your-ip-address:8080.


Connecting to the Server

Once the server is started, the web interface of the Concourse CI can be accessed by going to http://servers_public_IP:8080 in any browser.
Log in using the username 'test' and password 'test'.

To install the Concourse CLI (fly) on your system, click on the Linux logo to download, and run the following commands…

$ cd ~/Downloads/

$ install fly /usr/local/bin

$ which fly

> /usr/local/bin/fly

$ fly -v

> 6.2.0


Last step is to login. Using the fly login command will help accomplish this.

$ fly login -t hello -u test -p test -c http://servers_public_IP:8080 -u test -p test

> logging in to team 'main'

> target saved

Next step configure credentials file.

Create file credentials.yml on your concourse server. Exapmle for credentials file you can see at doc/credentials.yml

Next we need to setup our pipeline. Follow next command to setup pipeline:

We must go to folder with pipeline

$ cd /ci

$ fly -t ci set-pipeline -p pipelinename -c pipeline.yml --load-vars-from /path-to-file/credentials.yml

Now we launch our pipeline but pipeline is paused. To unpause pipeline you can go to web gui and unpused manualy or use next command:

$ fly -t ci unpause-pipeline --pipeline pipelinename


If you want to destroy pipeline use next command:

$ fly -t ci destroy-pipeline --pipeline pipelinename


If you want to check access for your resources you can use fly command:

$ fly -t ci check-resource --resource pipelinename/my_repo


Note: The -t flag is a target alias and is required for almost every command.
This alias comes in very handy when you’re targeting multiple concourse installations.

Setting up a Pipeline

Grabbing a bare bones Concourse pipeline is a good way to start kicking the tires. Luckily, there are lots of examples to choose from in the Concourse repos.

Note: If you need more examples for resource types you can visit https://github.com/concourse/concourse/wiki/Resource-Types


In our case we will use my pipeline which located ci/pipeline.yml

What this pipeline will do:

1. Get github repo
2. Create Dockerfile
3. Build docker image
4. Push docker image to aws ecr
5. Get our image and test him
6. Put to our deploy repo on ecr

We need to set up aws ecs and connect with our deploy repo.



# Pipeline result

![Scheme](https://bitbucket.org/scrumlaunch/aws_councourse_cypress_pipeline/raw/bab52a64b4f3041559d6f0ffd47a1f955c1581c1/doc/output.PNG)