# MyCustomGPTApp_guide
This is my attempt at creating a custom gpt app that is deployed using azure web apps 

Comments follow the delimiter "--"

Prerequisites: 
1. Docker Desktop --  Required to use docker engine for creating docker image and pushing it to the azure container store
2. Azure CLI      --  Azure CLI required to login into Azure from CMD and create and run azure resources from terminal

///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
Step 1:
1. Create a folder on local PC to hold the following files (use templates of the files from the customgptapp_tutorial_repo):
   a. appy.py            --  This will be the python script that will create and execute the application
   b. requirements.txt   --  This is a simple text file that will list all of the python packages required to runn the app.py file above. Include any specific package versions if applicable
   c. docker-compose.yml --  This is a yml file that will be used when creating web app which will specify how the docker image should be utilised when consumed by the app.
                         --   Important to change the "image" parameter in this file such that it has the following format --> <AzureCOntainerRegistryName>/azurecr.io/<DockerImageName>
                         --  Also include all environment variables that are required in the app.py script in this file
                         --  Lastly it is important to specify the port that the webapp will be served through. Please ensure that this port is the same as the one used/served in the app.py script. Example '80:8060'
   d. Dockerfile         --  The Docker file is the most important file that will provide the instructions for how the docker image will be created. This file should be copied from base docker image files and used only for the specific location. In this case we will be using a docker base file that if "FROM python:3.10-slim".  There are images available so search the correct one for the type of image you are trying to create.

STEP 2:
2.1 Once all the files have been added to the folder, we can now create the docker image as follows:
2.2 Open up the CMD (Command Terminal)
2.3 Create a docker image using the following command:

    docker build -t <ChooseContianerName> .      
    
-- Please note that you dont need to use the <>. It is also very important to specify the "." after the name of the container image as this tells docker to use the current working directory

2.4 ONce the docker image has been succesfully created, you can open docker desktop and check under "images" to see if it was created. Please note that you may experience errors whent trying to create the docker image. This will have to be troublshooted and resolved before the image can be successfully created. 

2.5 Once the image has been confirmed that it has been created the next step revolves around setting up the Azure cloud resources required for hosting the web app. 



    




