
# Magdalene: Cloud based Super Car E-commerce Website

Magdalene is a AWS cloud based and React ecommerce platform. Built with React, modular and fully customizable.


## Deployment on EC2

To deploy this project on Elastic Compute Cloud follow the steps.

Note:  
Add an inbound traffic rule (Custom TCP) for allowing port 3000.

To install nodeJS run

```bash
  sudo su -
  curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash
  . ~/.nvm/nvm.sh
  nvm install node
```
To confirm the installation run
```bash
  node -v
  npm -v
```

To update and install git run
```bash
  yum update -y
  yum install git -y
  git --version
```
Clone the repository:
```bash
  git clone https://github.com/Eamin05/CloudProject.git
  chmod +x /root/CloudProject/node_modules/.bin/nodemon
  cd CloudProject
  npm install
  npm start
```

The site will be visible once you copy and paste the "public_ip:3000".
## Deployment using CI / CD

Deploy this project using AWS services (CodeCommit, CodeBuild, CodeDeploy & CodePipeline).

Note:  
Create an Amazon S3 bucket to store the generated artifacts. For EC2 instance attach an IAM role to it with AmazonS3FullAccess & AWSCodeDeployFullAccess policies.

Steps:  

  1] Create a repository in AWS CodeCommit and upload all the files of this project in that repository.  
  Before creating a repository create an IAM user with AdministratorAccess and configure that role with your credentials (Git bash):

```bash
  aws configure
```

  Provide the details when prompted.  
  Choose the Security credentials tab. In the HTTPS Git credentials for AWS CodeCommit section, choose Generate.

```bash
  Create repository -> Magdalene-Repo(Any name will do) -> Clone URL (Clone HTTPS)
```

  Use the above generated credentials to clone the repository:

```bash
  cd Downloads
  git clone https://<your user name>:<your password>@git-codecommit.ap-south-1.amazonaws.com/v1/repos/Magdalene-Repo
```

  Add all the files to the newly created folder in downloads and run the following commands:

```bash
  cd Magdalene-Repo
  git init
  git status
  git add .
  git commit -m "First commit"
  git push origin master
```

  2] For building the project using AWS CodeBuild follow the steps:

```bash
  Create Project -> Magdalene(Any project name will do) -> AWS CodeCommit (Source provider) ->
  Select repository, branch and commitID -> Use a buildspec file -> buildspec.yml -> Artifacts ->
  Amazon S3 (Type) -> Select a bucket and give name for the artifact folder -> Zip (Artifacts 
  packaging) -> Create build project -> Start Build
```
Contents of buildspec.yml file:
```bash
  # Do not change version. This is the version of aws buildspec, not the version of your buldspec file.
  version: 0.2

  phases:
    install:
      runtime-versions:
        nodejs: 20 
      commands:
        - chmod 777 node_modules/.bin/nodemon
        - npm install
        - echo "Installing npm successfull"
    build:
      commands:
        - npm run
        - echo "Compilation successfull"
    post_build:
      commands:
        - echo Build completed on `date`
  artifacts:
    files:
      - '**/*'
    discard-paths: yes
```
  3] For deployment of project using AWS CodeDeploy follow the steps:

```bash
  Create Application -> Magdalene(Any name will do) -> EC2/On-premises (Compute platform) ->
  Create deployment group -> Magdalene-DG (deployment group name) -> Attach role -> Amazon EC2 
  instances -> Install AWS CodeDeploy Agent = Never -> Disable load balancing -> Create -> Create deployment
```

Note: For IAM role attach AmazonEC2FullAccess, AmazonS3FullAccess, & AWSCodeDeployRole policies to it. Also install CodeDeploy Agent on the EC2 instance with the following commands (make changes accoring to your region):


```bash
  yum install wget -y
  cd /home/ec2-user
  wget https://aws-codedeploy-ap-south-1.s3.ap-south-1.amazonaws.com/latest/install
  chmod +x ./install
  ./install auto
  systemctl start codedeploy-agent
  systemctl enable codedeploy-agent
  yum install -y httpd
  service httpd start
```

```bash
  Create deployment -> Copy and paste the Amazon S3 bucket where your artifacts is stored ->  Create
```

4] Using CodePipeline:
```bash
  Create pipeline -> Magdalene(Any name will do) -> Next -> 
  AWS CodeCommit (Source provider) -> Select repository & branch -> Next
  AWS CodeBuild (Build provider) -> Project name -> Next
  AWS CodeDeploy (Deploy provider) -> Select Application name & Deployment group -> Create
```

## Implementation

Architecture diagram:

![CloudAppFinal](https://github.com/Eamin05/CloudProject/assets/123094672/11290b09-7335-4ac4-a2ba-a21f65a6d95c)

Site (public IP):

![Screenshot 2024-05-18 142016](https://github.com/Eamin05/CloudProject/assets/123094672/8ca591a4-894b-4606-b43f-369e13bd71bb)

![Screenshot 2024-05-18 142200](https://github.com/Eamin05/CloudProject/assets/123094672/c16866ad-3fb3-44c8-a15a-87c44614974a)

![Screenshot 2024-05-18 142441](https://github.com/Eamin05/CloudProject/assets/123094672/825b3d45-54f4-4099-89ae-e1aedf22a284)

![Screenshot 2024-05-18 142547](https://github.com/Eamin05/CloudProject/assets/123094672/561c49f0-b6e0-4cc2-b21e-22c4ab7e78dd)

![Screenshot 2024-05-18 145823](https://github.com/Eamin05/CloudProject/assets/123094672/a05e250a-18b8-44a5-998d-a7098c2ab81f)

![Screenshot 2024-05-18 145906](https://github.com/Eamin05/CloudProject/assets/123094672/75686489-7bee-497d-b275-fb6880ed55a9)

![Screenshot 2024-05-18 145935](https://github.com/Eamin05/CloudProject/assets/123094672/e033df01-b0a3-466b-a689-47b30eccff24)

![Screenshot 2024-05-18 145950](https://github.com/Eamin05/CloudProject/assets/123094672/9eacc934-8502-4171-b264-8f07b16bed79)

![Screenshot 2024-05-18 151348](https://github.com/Eamin05/CloudProject/assets/123094672/be251ec7-5ccc-49ff-90c0-82880ed771fe)

![Screenshot 2024-05-18 152534](https://github.com/Eamin05/CloudProject/assets/123094672/6743186a-f0f9-4b29-b09d-2effd1ad7df6)

![Screenshot 2024-05-18 152547](https://github.com/Eamin05/CloudProject/assets/123094672/aeae9b2f-e69b-4e9d-8dfd-4c7999a4f878)

![Screenshot 2024-05-18 152605](https://github.com/Eamin05/CloudProject/assets/123094672/b91e7eeb-f107-4007-a8a5-008ae7b411f2)

![Screenshot 2024-05-18 160614](https://github.com/Eamin05/CloudProject/assets/123094672/98502022-cc50-4eb8-9d5d-dd21975b798b)

![Screenshot 2024-05-18 161216](https://github.com/Eamin05/CloudProject/assets/123094672/8589a8db-c28c-44eb-bafb-bd604270a877)

