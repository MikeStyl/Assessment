1.  Initial Learning and Setup:
    a.First, I watched some tutorials on how Docker works and followed the tutorials available in Docker Desktop.
    b.After completing those tutorials, I searched for instructions on how to install Jenkins inside a Docker container. I found an official Jenkins image 
    and used it for my project. After installation, I started the Jenkins instance on port 8080 and began the setup.

2.  Further Learning and Configuration:
    a.I continued my learning by watching more Jenkins tutorials, including ones that involved Docker integration. I watched a small playlist of such 
    tutorials.
    b.Following this, I created a pipeline in Jenkins and configured it to trigger a build on every commit. To set this up, I created a repository, placed 
    the necessary files inside it, and configured a webhook in the repository settings. However, I encountered an issue because Jenkins was running on 
    localhost, and GitHub cannot use a localhost URL for webhooks.

3.  Tunneling with Ngrok:
    a.To resolve the webhook issue, I researched ways to make it work. Many people suggested using programs that create a tunnel between localhost and a URL 
    that GitHub can use. I downloaded a program called Ngrok and used the command line to expose my localhost:8080/webhook. Ngrok provided me with a URL that  
    GitHub could use for events, which I entered into GitHub settings.

4.  Creating a Jenkinsfile:
    Next, I focused on creating a Jenkinsfile that could locate, Dockerize, and publish the image. Most tutorials recommended three main steps: fetching the 
    repository, building the image, and publishing it. To fetch the repository, I used the following Jenkins pipeline step:
    checkout scmGit(branches: [[name: '**']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/MikeStyl/Job-Project.git']])

5.  Configuring Docker Integration:
    While trying to build the Docker image with the docker.build step, I encountered an error indicating that the docker command was not recognized. To 
    resolve 
    this, I installed the Docker plugins and the Docker tool in Jenkins. I also assigned the tool the ID "Docker." Additionally, I added the "Initialize" 
    step.

6.  Permission Issues:
    Even after configuring Docker, I faced a new error stating, "Can't connect to Docker daemon." I researched this issue and found recommendations to mount 
    the docker.sock in the container when running it. I added the following command:" -v //var/run/docker.sock:/var/run/docker.sock"

7.  Resolving Permission Errors:
    While Docker was now detected, I encountered a permission error when trying to build. The error message was "Got permission denied when trying to connect 
    to the Docker daemon socket." To resolve this, I created a group and added both Docker and Jenkins to it. I used the exec command inside the container to 
    add them with the following: :usermod -aG docker jenkins

8.  Success with Docker Build:
    After some trial and error, I found that the command chmod 777 /var/run/docker.sock resolved the permission issue, but it posed security risks so I 
    avoided it.

9.  Continued Troubleshooting:
    I attempted various solutions found online and restarted Jenkins, but the error persisted. Eventually, I tried the chmod command, and it worked. I was 
    able to build the image, and only publishing remained.

10. Credential Configuration:
    To address a new error about missing credentials for publishing the image, I created a credential with my Docker Hub username and password and named it 
    "Docker HUB." I added this credential to the Jenkinsfile for use.

11. Installing Docker Inside the Container:
    However, I encountered a new error, "Cannot run program 'docker': No such program or file." Since forums did not provide a clear solution, I decided to 
    use the exec command inside the Docker container to install Docker. This allowed me to successfully build and publish the image.

12. Proxy Configuration:
    Moving on to setting up a proxy, I followed a tutorial and created a configuration file named nginx.conf. This file was configured to listen on port 80 
    on localhost and forward requests to hello_world:8080

13. Docker Compose:
    For Docker Compose, I created a docker-compose.yml file that included both the NGINX proxy and the hello_world app. I did not specify a port for 
    hello_world as I did not want it to be directly accessible. Using this configuration, when someone accessed port 80, it should redirect to hello_world.

14. Issues with Docker Compose:
    Unfortunately, I encountered an issue where accessing localhost:80 resulted in an "ERR_EMPTY_RESPONSE." This issue seemed to be related to how Docker 
    composed the services. When I tested hello_world individually by running python hello_world.py and accessing port 8080, it worked. However, when using 
    Docker Compose with both hello_world and the proxy, I received the "ERR_EMPTY_RESPONSE."


    
