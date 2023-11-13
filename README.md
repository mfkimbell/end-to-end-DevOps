## End-to-End DevOps

Visual Workflow
<img width="977" alt="Screenshot 2023-11-10 at 3 36 25 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/f5038967-6002-483d-ba4f-a2f213f2e9a0">

### **Tools Used:**
* `Maven` Managing dependencies and compiling Java code
* `Jenkins` Build and test automation of the application
* `Ansible` Deployment of the application
* `Tomcat` Manage server for Java application
* `Docker` Containerize the application and deploy server

SSH into an ec2 instance, and download jenkins and java (we create security groups that allow access from port 8080 for when we need to access our webapps

"service jenkins start" to start the jenkins server

access jenkins with public ip and port

   
<img width="639" alt="Screenshot 2023-11-11 at 5 52 00 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/22fc8cfa-0573-4cc2-aa7b-2628ce88f948">

Next, need to integrate github with jenkins

Install git on jenkins instance

Install github plugin on jenkins GUI

**Roadblock: jenkins uses "master" not "main" by default**

<img width="500" alt="Screenshot 2023-11-11 at 9 56 04 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/8e2e0385-9864-46c2-aa41-cfc0b5c768a6">

Confiure git on jenkins GUI

<img src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/f6cf2e22-05e8-4f6a-8bbd-915752163a42"  width=
"500">

Java JDK on Maven from our EC2 server

<img src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/ea7f7ba8-979c-4ba5-a0d9-da96edfd6b16" width="500" >
   
Integrate maven with jenkins

<img src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/0728c5e7-a434-426e-990a-c146e947649a" width="500" >

Here are the three Jenkins Jobs I've created

![Screenshot 2023-11-11 at 4 49 34 PM](https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/c28fcad5-c0bb-4188-bafd-bc8e78068a27)

Here we can see our build artifacts from the maven webapp job

<img src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/4360d000-df4a-4217-bb2d-4dad8e6fcb1e" width="500" >

Now we setup the Tomcat server similarly
    
Had to remove default permissions, and update it so we can have admin access. Also created some shortcuts for starting and stoppping the server

 <img width="863" alt="Screenshot 2023-11-11 at 6 10 35 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/3582e4a1-4067-4d7e-8f02-3e2c0d62a36b">

And then we login to the tomcat manager console

<img width="700" alt="Screenshot 2023-11-11 at 6 14 07 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/494d759c-0c28-475e-9130-bb13b6e51889">

Then we set up jenkins with some secrets to access the tomcat server

<img width="700" alt="Screenshot 2023-11-11 at 9 52 54 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/f828f35a-db47-4d59-bb97-75080f06b8c6">

Next we create a jenkins job "buildAndDeployServer" that builds the webapp on the tomcat server. After run, we see "webapp" now appears on the tomcat server.

<img width="700" alt="Screenshot 2023-11-11 at 10 13 44 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/d30cc4ce-297b-4591-ba3f-af66ca193214">

And now we can see our webapp running on Tomcat:

<img width="651" alt="Screenshot 2023-11-11 at 10 31 34 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/c1303646-ac7d-466c-ac6c-3ed26dab81d2">

But I ran this job MANUALLY. I want it to run every time I make changes to my code. So we set a build trigger for code changes in Jenkins. We can poll the software configuration management, which is a cron job that checks to see if the there are changes to the code, and only runs if there are changes. The current configuration I have set up uses a wildcard syntax to check the server every minute:

<img width="700" alt="Screenshot 2023-11-11 at 10 38 07 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/b8a0e8c9-e6cd-4b66-a873-df43c1fd465c">

And here is a job running automatically after a code change:



<img width="270" alt="Screenshot 2023-11-11 at 10 52 11 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/309f62a1-c3c6-489c-9881-ca31f949cb04">

->

<img width="375" alt="Screenshot 2023-11-11 at 10 43 09 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/2e3c248e-dbbb-4f25-bf78-48926aa63c06">




I want to deploy Tomcat into a docker container

So I make a new EC2 instance and install docker on it. 

We have the interal port as 8080, and the external port as 8081. So we need to update the security group of the docker-host. 

<img width="721" alt="Screenshot 2023-11-12 at 10 20 13 AM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/e748fdae-d813-428a-8bd4-4b0c6d181881">

Here we open ports 8081-9000:

<img width="731" alt="Screenshot 2023-11-12 at 10 22 27 AM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/5ad66480-3606-4703-912d-b10294907e99">

I write a dockerfile to install java and tomcat on centos and then start the Tomcat server. This was a nightmare to do for some reason. Normally you would just pull a Official Docker Tomcat image, but there was a well-known issue with that apparently:

<img width="7070" alt="Screenshot 2023-11-12 at 11 01 05 AM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/fdc1ea2e-74d6-46ee-bd91-765d2a284354">

Changed my docker container to be accessed via password so I can connect to it via jenkins. SSH is best practice, but this was easier.

<img width="7072" alt="Screenshot 2023-11-12 at 11 47 47 AM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/4b489f71-50a5-4908-9ec6-55069cbe6765">

Now we need to build a Jenkins job to deploy artifacts on our docker container. Specifically, we are sending the .war webapp file over SSH:

<img width="735" alt="Screenshot 2023-11-12 at 12 11 12 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/a6745334-e784-4b13-b490-59702fe25c85">

Spent a couple hours experimenting here since I again ran into some issues:

<img width="563" alt="Screenshot 2023-11-12 at 1 07 29 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/033a636b-7137-4f5d-8751-a7d1f5570824">

Finally managed to get the webapp.war file on my docker instance

<img width="599" alt="Screenshot 2023-11-12 at 1 23 42 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/ff8b7661-8f35-484c-b9df-a4d30016a680">

Now we create a new folder to copy the artifacts to in the EC2 instance:

<img width="682" alt="Screenshot 2023-11-12 at 1 39 47 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/2a361316-80d4-471d-8fef-fa9d295805a4">

We also update our Jenkins job to reflect that:

<img width="542" alt="Screenshot 2023-11-12 at 1 41 58 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/d175fb11-3b3c-45d0-8f35-1d693650079a">

Now that the file is with our dockerfile, we can update it to send the .war file to the tomcat webapp folder. 

<img width="633" alt="Screenshot 2023-11-12 at 1 45 20 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/d37b8a0c-1eae-48b2-9064-971351162308">

Create a new container with these updates:

<img width="650" alt="Screenshot 2023-11-12 at 1 48 32 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/8074b5c2-d5b2-4f72-afef-cd14b8fd9959">

Now  if we go to the URL, (publicIP):8086/webapp we can see our webapp:

<img width="635" alt="Screenshot 2023-11-12 at 1 49 39 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/25d0d6f5-da32-4882-a4e9-cf7582a14e8b">

However, I built this container manually, and I'd like jenkins to build it for me. So we set it to check for updates every minute, and remove and rebuild if there are changes to the code:

<img width="538" alt="Screenshot 2023-11-12 at 2 12 29 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/57299653-db93-4d41-8802-d77b180518a3">

Now we are going to integrate Ansible as a deployment tool. We set up access from our Ansible EC2 to the Docker EC2 with it's private IP since we are in a VPC, so that's allowed:

<img width="546" alt="Screenshot 2023-11-12 at 3 25 39 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/abfe1103-ae30-4cd4-9d55-943d25b41ef2">

We can see that Ansible was successfully able to acces the private IP:

<img width="874" alt="Screenshot 2023-11-12 at 3 58 41 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/e0a0e0de-bd87-4856-adfa-5174fc1ae1ff">

Now we need to make it so Jenkins can integrate with Ansible. We need to make a new Jenkins job that this time excludes the "exec" jobs, since we will be handling the deployment on Ansible.

<img width="918" alt="Screenshot 2023-11-12 at 4 27 27 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/cd810e4b-4ccb-4e1b-ad94-8bad043b37e9">

We give our Anisble EC2 it's own dockerfile with the same instructions as before on building the Tomcat server, build that image, and then build that container. 

<img width="828" alt="Screenshot 2023-11-12 at 4 48 32 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/a960d07d-1261-4efe-89ab-69369ab90b07">

And the webapp sucessfully runs on the docker container in the Ansible EC2:

<img width="538" alt="Screenshot 2023-11-12 at 4 50 43 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/78a0d960-1cd2-4d22-a7fe-a596dba842d5">

Now we need to create an Ansible Playbook to execute these commands automatically. We need to instruct Ansible to instruct the Docker container to pull a container from DockerHub. First off, we need to change the host IPs on the Ansible server so we can access a playbook file on the Ansible server, altering vi/ansible/hosts. We create groups based on where we want the execution to occur:

<img width="592" alt="Screenshot 2023-11-12 at 5 01 53 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/6b706ac2-9a6b-496c-a5bd-b705e58177fe">

This is my basic playbook. I create a task to create an image and specify that if this playbook is available it can be executed in the docker directory. 

<img width="433" alt="Screenshot 2023-11-12 at 5 17 45 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/3677e783-742d-4381-8480-92ff5fd1780e">

After running the task, we can see the image has been added:

<img width="944" alt="Screenshot 2023-11-12 at 5 20 24 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/10eb9d93-ec0d-4483-8d07-09245422fbc5">

Now, we want to upload our image onto DockerHub. So I login to DockerHub on my Ansible EC2, and then manually push the image of interest. 

<img width="758" alt="Screenshot 2023-11-12 at 5 35 11 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/051e4e84-5f4c-46e7-ab9b-a474c103b80f">

And now we see it on DockerHub:

<img width="963" alt="Screenshot 2023-11-12 at 5 35 48 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/36774ef0-1ec5-4f69-934a-100e8157da9e">

Now I'd like to incorporate these steps into Ansible Playbook. I create two new tasks on the playbook to tag and upload the image:

<img width="572" alt="Screenshot 2023-11-12 at 5 40 45 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/a5f7b9a2-6ad7-487c-86ee-4a700e02b2b6">

Now I alter my jenkins job to execute the Ansible Playbook, as well as adding a "Poll ACM: * * * * *" so it's run automatically with code changes:

<img width="545" alt="Screenshot 2023-11-12 at 5 51 07 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/82072d28-0c0e-4d20-86a7-b03209c94393">

Next, I create another playbook, deploy_regapp.yml, that graps the container from DockerHub.

<img width="799" alt="Screenshot 2023-11-12 at 6 24 21 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/b7a588c2-a132-4357-ac49-811f33353f69">

And once again, the webapp is up and running, this time through Ansible Playbook.

<img width="550" alt="Screenshot 2023-11-12 at 6 34 38 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/52ce3440-34d9-42e9-95eb-bcf7becd021e">

Next, we need to update our playbook to remove current versions so that the container actually gets updated when there are changes. 

<img width="765" alt="Screenshot 2023-11-12 at 6 43 28 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/be6c3646-3e2b-4f63-bfd2-ccdfc9c3531f">

And we add that playbook to the Jenkins job execs:

<img width="537" alt="Screenshot 2023-11-12 at 6 49 04 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/14fbc7ab-c0c6-4adc-ab61-ef44c6565a43">

Finally, our webapp is up and running again, all done automatically through Jenkins and Ansible whenever a change is made.

<img width="480" alt="Screenshot 2023-11-12 at 6 53 08 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/d69a6c73-201a-463b-870e-5f8fb883356e">

Here are all of the Jenkins jobs so far, Copy_Artifacts_onto_Ansible is the recent one I was referring to:

<img width="707" alt="Screenshot 2023-11-12 at 6 58 13 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/b1855a5f-0d38-4c45-b129-4c4f8c81076d">

