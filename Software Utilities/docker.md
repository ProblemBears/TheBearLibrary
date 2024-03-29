<h1 align="center"> Docker </h2>
<h2 align ="center"> DOCKER IMAGES & CONTAINERS: THE CORE BUILDING BLOCKS </h2>

### Images and Containers				
- Images are the blueprints for containers, the contain the code + required tools/runtimes
- What and Why?					
    * Containers are the running "unit of software"
    * So IMAGES are used to create MULTIPLE CONTAINERS

### Using and Running External (Pre-Built) Images			
- We can Find images
	* Coworkers hand it to us
	* DockerHub - Where we can search community images
	* **EX :** 	
		* We can run NodeJS's image via a container by using the command:
			```docker
			docker run node
			``` 
			(anywhere in terminal) which runs a container and pulls the image if not already locally installed.

		* The program is running but since containers isolate it inside, we can't see it. To verify then, we do `docker ps -a` **to see all processes**
		* `docker run -it node` to actually INTERACT with the container's process

### Node.js App - Building Our Own Image with a Dockerfile					
- Typically we build on top of or create an image based on another image (such as those on the DockerHub)
- For example, normally, for a Node.js app. We would npm install dependencies and then do "node app.js" to run our app (which may be a server)
- Instead, with Docker, we can create our own IMAGE that contains our app AND ALSO utilizes the NODE.JS Image. To do this we do the following:
	1. In your App's root directory: **Create a file called "Dockerfile"** (The VSCode Docker Extension helps)
		- The "Dockerfile" - contains the instructions for Docker that we want to execute when we build our own image.
	2. Write the following code in "Dockerfile" (w/ explanations):
		- `FROM node`				
			* We build on an image that exists on our system or the DockerHub
			* "Hey Docker, in my own image I want to start by pulling the `node` Image.

		- `COPY . /app`				
			* `COPY` - Which files that are in our local machine should go in the external Image
			* `.`  - **Source** to be copied, in this case . refers to the directory where the Dockerfile is located (our App's entire contents except for the Dockerfile)
			* `/app` - Destination of the copy, where . refers to the root directory inside of the FROM'd image (Every image/container) has its own internal filesystem
				* It's actually recommended to put it in some subfolder so here we replace "." with "/app (an absolute path)"

		- `WORKDIR /app`				
			* Sets the working directory to the FROM'd Image's app directory where we copied our App files into previously.
			* If we had set this before the COPY then we could've just used "COPY . ."

		- `RUN npm install`				
			* NOW because we're in the correct working directory where we copied our files into we can run "npm install" successfully.
			* This runs when the Image is created

		- `EXPOSE 80`				
			* "When this container is started, expose port 80 to our local system"
			* This is necessary because since Docker containers are isolated, they even have their own ports, which is why we have to "expose them" to our system ports

		- `CMD ["node", "server.js"]`		
			* &cross; WE CAN NOT do RUN `node server.js` because the Dockerfile serves as instructions of how to setup an Image THE CONTAINER IS THE THING WE RUN!!! Instead, To tell a container to "run" something when it starts we use `CMD` (which works exactly like `RUN` but has weird syntax)
			* **This should always be the last command in your Dockerfile**

### Running a container based on our own Image				
- To create the Image :	  
Terminal &rarr; Directory Where "Dockerfile" resides &rarr; `docker build .`

- To run the newly created image :  
	- `docker run <image-id>`

- To shut down a running container :
	- `docker ps` - To show the running containers & their info
	- `docker stop <container-name>` - might take a while

- To actually make the image run on a PORT :		
	- `docker run -p <port>:<docker_exposed_port> <image_id>` 


### Images are read only				
- We have to rebuild our image everytime we change our code because the COPY command is what actually updates the Image. (There is a faster way later)

### Understanding Image Layers			
- Docker caches every instruction in the Dockerfile, therefore, when we rebuild it only executes instructions that need to re-execute (ex: Only `Copy` executes when you change your code and rebuild). This is what is meant by layer based.

### Stopping & Restarting Containers			
- `docker |command|--help`
- `docker start <container_name>` -	to restart an "Exited" container (in contast to the "run" command which starts a new container)

### Understanding Attached & Detached Containers
- **"Attached"** refers to the Container blocking your terminal sesssion when you run it it, because it serves as a log for your container
- By default, **"run" is in attached mode**, and **"start" is detached**
- To `run` in detached mode `-d` flag :
	* `docker run -p 8000:80 -d <image_id>`
- To `start` in attached mode `-a` flag :
	* `docker start -a <container_name>`
- To attach yourself to a detached container :
	* `docker attach <container_name>`
- To get container logs without attaching :
	* `docker logs <container_name>`
	* `docker logs -f <container_name>` 	// to "follow" aka attach

## Entering Interactive Mode			
- (Switching to Python) we have the scenario where the code states that user input will be required so we have to make our Docker Container "interactive" :
	1. Dockerfile for Python :
		```docker
		FROM python

		WORKDIR /app

		COPY . /app

		CMD [ "python", "rng.py"]
		```

	2. Generate an Image that runs in interactive mode and w/ a tty terminal :
		* `docker run -i -t <image-id>`		//-i and -t can be shorthanded to -it for interactive mode terminal tty.

	3. The process stops when the input is put in. We can re-"start" it in attached mode, but A TIDBIT IS that when we 
		generated the image with "run -i -t", the -i flag is forgotten so we have to respecify it when we "start" as follows:
		* `docker start -a -i <container_name>`

### Deleting Images & Containers			
- Delete a container
	* `docker stop <container-name>` 		//if the container is running
	* `docker rm <container-name1> <container_name2>` //etc...

- Delete all stopped containers
	* `docker container prune`

- Display all images
	* `docker images`
- Remove images
	* `docker rmi <image_id1> <image_id2> //etc...` //You can only remove images that don't have containers (even stopped ones)
- Remove all images that don't have running containers:
	* `docker image prune`

### Removing Stopped Containers Automatically			
- `docker run -p 3000:80 -d --rm <image_id>`
	* The `--rm`  flag removes containers automatically after stopping

### A Look Behind the Scenes - Inspecting Images			
- Multiple Containers using the same Image share the code given by the Image. They make their own thin layer above the image where they can change things (like their own filesystem)
- To inspect an image :
	* `docker image inspect <image_id>`

### Copying Files into & From a Container				
- Copy files/folders into or out of a running container :
	* `docker cp <sourcePath>/. <container-name>:/<containerDestPath>` - where "`/.`" copies all files in that sourcePath of you local machine
	* reverse the source and dest paths to copy from a container to the local machine

### Naming & Tagging Containers and Images
- When you run a container you can name it with :
	* docker run -p 3000:80 -d --rm --name <new_name> <image_id>

- To name(tag) an image while building :
	* `docker build -t goals:v1 .`
	* Convention says tags should be named in this format :	`repoName : tag	(ex: node:v14 )`
		* DOCKER knows this format so it'll set the REPOSITORY and TAG fields when we search up all docker images
- After giving a Docker Image a `Repo:Tag` , we can use that to run containers and not have to use `<image_id>`

### Sharing Images - Overview			
1. We can share the whole directory that includes the Dockerfile so that people can run the Docker commands to build an image out of it
2. We can share a built image: we just download it and run it

### Pushing Images to Dockerhub			
- Sign up for DockerHub
- Share an image :
	* DockerHub: Repositories > Create Repository
	* Name the image to match the DockerHub repo image :
		* By running it again
		* By renaming an old image : `docker tag <old_name:tag> <new_name:tag>`
	* To allow the connection to DockerHub :  
		```bash
		docker login
		docker push <image_name>
		```
- Use an image :  
	`docker pull <image_name>`