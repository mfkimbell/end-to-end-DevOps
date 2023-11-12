## End-to-End DevOps

Visual Workflow
<img width="977" alt="Screenshot 2023-11-10 at 3 36 25 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/f5038967-6002-483d-ba4f-a2f213f2e9a0">

### **Tools Used:**
* `Maven` Managing dependencies and compiling Java code
* `Jenkins` Build and test automation and deployment of test enviornments
* `Tomcat` Manage server for Java application


1. ssh into an ec2 instance, and download jenkins and java (we create security groups that allow access from port 8080 for when we need to access our webapps


3. "service jenkins start" to start the jenkins server
4. access jenkins with public ip and port

   
<img width="639" alt="Screenshot 2023-11-11 at 5 52 00 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/22fc8cfa-0573-4cc2-aa7b-2628ce88f948">

6. next, need to integrate github with jenkins
7. install git on jenkins instance
9. install github plugin on jenkins GUI
10. Roadblock: jenkins uses "master" not "main" by default  

<img width="1159" alt="Screenshot 2023-11-11 at 9 56 04 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/8e2e0385-9864-46c2-aa41-cfc0b5c768a6">

8. confiure git on jenkins GUI

![Screenshot 2023-11-11 at 4 32 43 PM](https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/f6cf2e22-05e8-4f6a-8bbd-915752163a42)

9. Java JDK on Maven from our EC2 server

![Screenshot 2023-11-11 at 4 32 34 PM](https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/ea7f7ba8-979c-4ba5-a0d9-da96edfd6b16)
   
10. integrate maven with jenkins

![Screenshot 2023-11-11 at 4 34 51 PM](https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/0728c5e7-a434-426e-990a-c146e947649a)

11. Here are the three Jenkins Jobs I've created

![Screenshot 2023-11-11 at 4 49 34 PM](https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/c28fcad5-c0bb-4188-bafd-bc8e78068a27)

12. Here we can see our build artifacts from the maven webapp job

![image](https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/4360d000-df4a-4217-bb2d-4dad8e6fcb1e)

13. Now we setup the Tomcat server similarly
    
15. Had to remove default permissions, and update it so we can have admin access. Also created some shortcuts for starting and stoppping the server

 <img width="863" alt="Screenshot 2023-11-11 at 6 10 35 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/3582e4a1-4067-4d7e-8f02-3e2c0d62a36b">

 16. And then we login to the tomcat manager console

<img width="1647" alt="Screenshot 2023-11-11 at 6 14 07 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/494d759c-0c28-475e-9130-bb13b6e51889">

17. Then we set up jenkins with some secrets to access the tomcat server

<img width="1423" alt="Screenshot 2023-11-11 at 9 52 54 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/f828f35a-db47-4d59-bb97-75080f06b8c6">

Next we create a jenkins job "buildAndDeployServer" that builds the webapp on the tomcat server. After run, we see "webapp" now appears on the tomcat server.

<img width="1689" alt="Screenshot 2023-11-11 at 10 13 44 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/d30cc4ce-297b-4591-ba3f-af66ca193214">

And now we can see our webapp running on Tomcat:

<img width="651" alt="Screenshot 2023-11-11 at 10 31 34 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/c1303646-ac7d-466c-ac6c-3ed26dab81d2">

But I ran this job MANUALLY. I want it to run every time I make changes to my code. So we set a build trigger for code changes in Jenkins. We can poll the software configuration management, which is a cron job that checks to see if the there are changes to the code, and only runs if there are changes. The current configuration I have set up uses a wildcard syntax to check the server every minute:

<img width="1089" alt="Screenshot 2023-11-11 at 10 38 07 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/b8a0e8c9-e6cd-4b66-a873-df43c1fd465c">

And here is a job running automatically after a code change:
<img width="270" alt="Screenshot 2023-11-11 at 10 52 11 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/309f62a1-c3c6-489c-9881-ca31f949cb04">

<img width="375" alt="Screenshot 2023-11-11 at 10 43 09 PM" src="https://github.com/mfkimbell/end-to-end-DevOps/assets/107063397/2e3c248e-dbbb-4f25-bf78-48926aa63c06">

