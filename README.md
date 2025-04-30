# Haunted-places-dataset-image-forensics-and-similarity-analysis-using-NASA-JPL-ImageSpace

<br>Author: Sena London, MS APDS Student
<br> Open Sources : Copilot, ChatGPT and other tools
<br>https://github.com/nasa-jpl-memex/image_space


### Memex ImageSpace Deployment Steps

This guide assume image_space folder is inside the root directory

####  Run SMQTK Image Services

✅This step starts the SMQTK services for image processing such as feature extraction.
<br>cd image_space/imagespace_smqtk
<br>./smqtk_services.run_images.sh --docker-network deploy_imagespace-network --images /root/image_space/images

- `--docker-network deploy_imagespace-network`: Uses the specified Docker network.
- `--images /root/image_space/images`: Directory containing the images to process.

If the network doesn't exist yet, create it with:

```bash
docker network create deploy_imagespace-network
```

####  Deploy Core Services Using Docker Compose

✅Launch the backend services like Solr, Redis, and Django:
cd image_space/scripts/deploy
IMAGE_DIR=/root/image_space/images docker-compose up -d

'''Shut down'''
cd image_space/scripts/deploy
IMAGE_DIR=/root/image_space/images docker-compose down

- `IMAGE_DIR=/root/image_space/images`: Specifies where the images are mounted inside the containers.
- `docker-compose up -d`: Runs the containers in detached mode.

Ensure you have an appropriate `.env` file or override configurations if required.
---

✅Create the Docker Network if the imagespace-network doesn’t exist, create it manually
docker network create imagespace-network


✅Redeploy Core Services Using Docker Compose after recreating network
cd image_space/scripts/deploy
IMAGE_DIR=/root/image_space/images docker-compose down
IMAGE_DIR=/root/image_space/images docker-compose up -d


✅Verify Running Containers
docker ps

# You should see containers like:
deploy-imagespace-mongo-1
deploy-imagespace-solr-1
deploy-imagespace-girder-1
deploy-imagespace-imagecat-1



Enable ImageSpace : Run the setup script to finalize the configuration:
✅Run the setup script to finalize the configuration:
cd ~/image_space/scripts/deploy
sh ./imagespace/enable-imagespace.sh

# Check Docker Network: If you face issues related to the Docker network, verify it exists:
docker network ls

# If the imagespace-network is missing, create it again:
docker network create imagespace-network

✅Troubleshooting

- **Check Running Containers**:
docker ps

- **View Logs if Troubleshooting**:
docker-compose logs -f

✅  Verify all containers are running
docker ps

#####

Run:

docker-compose up -d --force-recreate
Wait 15 to 30 seconds, then check:
docker ps

Make sure all required containers (mongo, solr, girder, imagecat) are up.

Then run:
cd ~/image_space/scripts/deploy
sh ./imagespace/enable-imagespace.sh

✅ Now that you have set up your containers and run the necessary setup script, the next steps are typically:

Verify that all services are running: Ensure that all containers are up and running by checking the status with:
docker ps
This should show you the containers related to ImageSpace, such as imagespace-girder, imagespace-solr, imagespace-mongo, etc., as running.

Check the logs for any issues: If any container is not starting correctly, check its logs for detailed error messages:
docker logs <container_name>
For example:
docker logs imagespace-girder

✅ Test the environment: Once the services are confirmed to be up:
Girder should be accessible on the port mapped in your docker-compose.yml (e.g., port 8989).
Solr should be accessible at port 8983.
MongoDB should be running correctly.
Test ImageSpace functionality: You can now access the ImageSpace web interface through the mapped port (e.g., http://localhost:8989 or similar) and ensure everything is working.

✅ http://localhost:8989

![image](https://github.com/user-attachments/assets/9a034f0f-2d85-4a5c-abf2-a7beb13d4548)


If there are any errors or issues along the way, check the logs and address any dependencies or configurations that might be missing.

✅This show you the images that are located in the IMAGE_DIR directory on your host machine:
docker exec -it deploy-imagespace-imagecat-1 /bin/bash

Once inside the container, navigate to the /images directory and list the files:
ls /images

