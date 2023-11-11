## DevOps Project for Beginners   

1. ssh into an ec2 instance, and download jenkins and java
2. "service jenkins start" to start the jenkins server
3. access jenkins with public ip and port
4. next, need to integrate github with jenkins
5. install git on jenkins instance
6. install github plugin on jenkins GUI
7. Roadblock: jenkins uses "master" not "main" by default  

Went into the EC2 server, started the jenkins server, then added files to support git, java and maven

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
