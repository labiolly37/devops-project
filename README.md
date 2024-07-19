This documentation is how to provision an EKS cluster with Terraform, use Github Action to build and tag an image and deploy the image to EKS Cluster with Helm. 

**PROVISION AN EKS CLUSTER WITH TERRAFORM****
Step 1: Provision an EC2 Instance, ssh into it and update 

Step 2: Install git, docker, terraform and helm and confirm that they are all running. 
![Screenshot 2024-07-18 190331](https://github.com/user-attachments/assets/c8ea062d-a8c0-4908-b9eb-a6fbfec56f2e)

Step 3: Git clone the repository where your codes are
![Screenshot 2024-07-18 190708](https://github.com/user-attachments/assets/495c38a8-8ef9-48ff-8b16-e1fbc111b9d3)

Step 4: You can delete the /helm-chart and /.github, folders. 

Step 5: Switch to /helmchart folder in your codebase workdir and execute the command:

helm create docker-cicd
![Screenshot 2024-07-18 191329](https://github.com/user-attachments/assets/7cc30e39-5f64-4a89-86a2-98b158ffb6a0)

By default, helm will initialise a chart that deploys nginx. In our case, we need to deploy our own image not nginx. To do this, edit the values.yaml file in line 7, to 11, edit to the code snippet below. The code block, removes nginx image and replaces it with username/imagename. Don’t worry about it being invalid the plan is to get the pipeline to automatically fill in the username, image name, and tag name so just let the username and imagename be like that, don’t edit it to your own
![Screenshot 2024-07-18 191932](https://github.com/user-attachments/assets/62557468-a394-49fb-bbf5-95229d3b8d58)

Step 6: Provision EKS with Terraform: 
If you already have an EKS cluster running in AWS you can use, you can skip this part and jump straight to the GitHub actions section. Else, navigate to /terraform folder in the codebase directory and execute the command:

terraform init
Note: Ensure you have set up your AWS CLI and access keys on your local machine before executing the terraform commands.

Next, apply the terraform script:

terraform apply --auto-approve

The terraform script will create the following:
VPC
Private & Public Subnets
EKS Cluster
1 t2.medium node
Note: The terraform script will take some time to provision approximately ~10–15min
![Screenshot 2024-07-18 121955](https://github.com/user-attachments/assets/e104b3ff-ef67-490a-a42c-1e04ca673dd2)
![Screenshot 2024-07-18 122016](https://github.com/user-attachments/assets/01b58ad6-c969-4068-b746-69d0ffd405eb)
![Screenshot 2024-07-18 125710](https://github.com/user-attachments/assets/448bc9a4-0a1a-4cf2-9fff-b897614a482c)

Confirm from the AWS CONSOLE:
![Screenshot 2024-07-18 125830](https://github.com/user-attachments/assets/2154c9ce-7069-4ee3-90b6-dab3eefec507)

Step 7: CICD Pipeline: In the project work directory, create a folder /.github/workflows don’t forget to add the dot before the github. In the workflows folder, create a yml file as shown below:
![Screenshot 2024-07-18 193259](https://github.com/user-attachments/assets/d7d954a3-bdef-4a09-bb85-e038fb0b7924)
![image](https://github.com/user-attachments/assets/faa24268-d613-4af3-b54b-435fba914a91)

STEP 8: Save your secrets/variables in your GitHub for Github action pipeline to use. 
Click on New Repository Secret. Your secret name should match the below

AWS_ACCESS_KEY_ID — Your aws access key

AWS_SECRET_ACCESS_KEY — Your aws secret access key

DOCKERHUB_TOKEN — Add your dockerhub password/ token

DOCKERHUB_USERNAME — Add your dockerhub username
![Screenshot 2024-07-18 193621](https://github.com/user-attachments/assets/c98f4028-6305-4e2b-9238-563ee356a6fd)

Step 9: After adding the repository secret, push the changes you have locally and check the actions tab for the pipeline trigger.
![Screenshot 2024-07-18 183940](https://github.com/user-attachments/assets/3bbe1f86-66aa-4b0f-8817-3af2fa2c89c7)
![Screenshot 2024-07-18 184044](https://github.com/user-attachments/assets/59b5c78d-cb9e-4c96-86e0-4ad5f2845926)

If successful, you should have a new image tag in docker hub and the app running in kubernetes.

Install kubectl in your terminal to interact with your EKS Cluster.
![Screenshot 2024-07-18 194413](https://github.com/user-attachments/assets/d776a376-c1f2-4a23-b94c-ae439323bf7d)

Step 10. Use the below command to dd a new context and interact withyour Cluster;
![Screenshot 2024-07-18 194527](https://github.com/user-attachments/assets/d0ec71c3-bcba-438e-9e9c-6fd67dcd0dcc)

Step 11: Confirm by getting pods or nodes;

![Screenshot 2024-07-18 194745](https://github.com/user-attachments/assets/8417b040-b243-40aa-a3e2-3965c1d25c8d)

You'll notice that the newly built image (note the release name supplied in the build.yaml file) has been deployed to the EKS Cluster. 
![Screenshot 2024-07-18 195053](https://github.com/user-attachments/assets/90c63474-d2bd-48ca-96f5-85297f15b1d6)

Then this...

![Screenshot 2024-07-18 195149](https://github.com/user-attachments/assets/98edffda-4529-4b60-83b3-02fea2ed1c41)











