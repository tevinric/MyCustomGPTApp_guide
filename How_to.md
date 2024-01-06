# MyCustomGPTApp_guide
This is my attempt at creating a custom gpt app that is deployed using azure web apps 

Comments follow the delimiter "--"

Prerequisites: 
1. Docker Desktop --  Required to use docker engine for creating docker image and pushing it to the azure container store
2. Azure CLI      --  Azure CLI required to login into Azure from CMD and create and run azure resources from terminal

///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
Step 1:
1. Create a folder on local PC to hold the following files (use templates of the files from the customgptapp_tutorial_repo):
   
   a. appy.py:
                              This will be the python script that will create and execute the application.
   
   b. requirements.txt:      This is a simple text file that will list all of the python packages required to runn the app.py file above. Include any specific package versions if applicable
   
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

STEP 3:

3.1 We will now need to login to the Azure portal to set up the environment needed for creating and hosting our webapp. This can be done through the Azure web portal or through the Azure CLI commands. We will use the Azure CLI commands after having it installed on our PC.

3.2 RUn the following line of code on the CMD Terminal to log into Azure. THis will ask for validation.

   az login

3.3 In Azure all resources are created under resource containers which are allocated to subscriptions. The heierachy is as follows:

   Subscription > ResourceGroups > Resources

   There can be multiple resources allocated to a single resource group as well as multiple resource groups under a single subscription

3.4 Create a Resource group using the following command in CMD:

   az group create --name <ChooseResourceGroupName> --location eastus

   --Location can be changed to match a nearby region

3.5 Set the plan for the Resource Group and create an Azure Conatainer registry, we will use a basic plan for most cases:

   az acr create --resource-group <ChooseResourceGroupName> --name <ChooseAzureContainerRegistryName> --sku Basic

   --his is required to host our docker images which get stored in containers
   
3.6 Now log into the desired container registry:

   az acr login --name <ChooseAzureContainerRegistryName>

3.7 Now in the same CMD terminal session in which we created the docker image, we must now create a tag for the docker image so that we can get it ready for pushing to the AzureContainerRegistry.

   docker tag <ChooseContianerName>  <ChooseAzureContainerRegistryName>.azurecr.io/<ChooseContianerName>

3.8 Now that the container is ready, we can push the docker image into the AzureContainerRegistry in Azure:

   docker push <ChooseAzureContainerRegistryName>.azurecr.io/<ChooseContianerName>

STEP 4: 

4.1 We will now need to set up the environment for hosting our web app. The first step involves creating an Azure App Service Plan which will define the compute and memory that we will require to run the web app. You can create a plan to meet the budget allocation and play with scalable plans that will allow for the compute to be elastic in respone to user load. 

4.2 Once you have a plan created, you can now create an Azure webapp that will be linked the the Azure WebApp plan. The plan will provision the resources required for the app to run. 

STEP 5:

5.1 The final step of the process is to create the azure web app that will be used to host the application.

5.2. Use th following line of code in the CMD terminal to create the Azure WebApp: 

   az webapp create --resource-group <ChooseResourceGroupName> --plan <AzureAppPlanName>--name <ChooseWebAppName> --multicontainer-config-    type compose --multicontainer-config-file docker-compose.yml

-- The above line of code will reference the "docker-compose.yml" file in the directory folder to get the settings on how the file should be created. 
-- This file will have information regaring which AzureContainerRegistry to use for the docker image that we need to run.
-- It will also host information about the port that the app should be served in. Remember that the port must be consistent through all the files created for the application to work.


   
   
   






    




