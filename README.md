# 1.0 Configuration to run OpenMRS and KenyaEMR

Installation of Tomcat9, Java8 and Mysql configuration on ubuntu 20.04

## 1.1 Installing Java8 on ubuntu
first, Before anything check if you have any java version install on the ubuntu machine or server before installing it.

#### 1.1.0 To open terminal
      ctrl + alt + t 
      
#### 1.1.1 Check for any Java Versions
      sudo update-alternatives --config java

#### 1.1.2 Uninstall the java version present part from java 8
      sudo apt-get remove <package_name>

#### 1.1.3 To remove the configuration files of unwanted java<_version_>
      sudo apt-get purge <java_version>

#### 1.1.4 Finally, you can check if the Java version has been completely removed
      java -version
      
#### 1.1.5 Update the ubuntu Package Manager
      sudo apt-get update
     
#### 1.1.6 Now, install Java8 on your machine
      sudo apt install openjdk-8-jdk

#### 1.1.7 Again, Update the ubuntu Package Manager after installing java8
      sudo apt-get update
      
#### 1.1.8 Confirm the java version after installing
      java -version
      
## Java Version Screenshot:
![Screenshot from 2023-03-13 19-25-55](https://user-images.githubusercontent.com/67967749/224764778-52ac99c8-d3da-4551-b21d-f7dce72fc5d2.png)

## 1.2 Installing tomcat9 on ubuntu
We are going to remove any existing tomcat version and install the correct version tomcat9
#### 1.2.0 To open terminal
      ctrl + alt + t 

#### 1.2.1 To Stop any existing tomcat running
      sudo service tomcat6 stop
      
#### 1.2.2 To remove any files from tomcat6
      sudo apt-get autoremove --purge tomcat6
      
#### 1.2.3 To remove any configuration file
      sudo apt-get autoremove --purge tomcat6
      
#### 1.2.4 Update the ubuntu Package Manager after removing tomcat6
      sudo apt-get update
      
#### 1.2.5 Now, Install TomCat9
      sudo apt install tomcat9
      
#### 1.2.6 Install extra libraries from tomcat9
      sudo apt install tomcat9-docs tomcat9-examples tomcat9-admin
      
#### 1.2.4 Again Update the ubuntu Package Manager after install tomcat9
      sudo apt-get update

#### 1.2.4 After Successully installation, Check the Tomcat9 status
      sudo service tomcat9 status
      
After Checking TomCat9 Status Screenshot:

![Screenshot from 2023-03-13 19-45-04](https://user-images.githubusercontent.com/67967749/224769148-a8bc1413-226b-456a-af25-db7ac086f6b4.png)

## 1.3 Configuration of TomCat9 to run OpenMRS and KenyaEMR
Now, we are going to configure the tomcat config,

#### 1.3.0 To open terminal
      ctrl + alt + t 

#### 1.3.1 Lets nano server.xml. We, are setting connector to 200MB 
      sudo nano /etc/tomcat9/server.xml
      
###### copy and paste the following as shown on screenshot:
          maxPostSize= "209715200"
          URIEncoding="UTF-8"
          relaxedQueryChars="[,]"
          
After Adding lines to Connector to 200MB Screenshot:

![connector2](https://user-images.githubusercontent.com/67967749/224772118-223e670a-b552-4590-94a1-9b09d0f054e8.png)

      
#### 1.3.3 To Configure Multipart Config from web.xml file:
      sudo nano /usr/share/tomcat9-admin/manager/WEB-INF/web.xml
      
###### copy and paste the following as shown on screenshot:
          <max-file-size>209715200</max-file-size>
          <max-request-size>209715200</max-request-size>
          
After adding the lines to Multipart Config Screenshot:

![multipart](https://user-images.githubusercontent.com/67967749/224774965-ca576d37-68c2-443d-83d7-36ebf1ad843c.png)

#### 1.3.4 Cd to catalina.properties
           sudo nano /var/lib/tomcat9/conf/catalina.properties
           
###### add this lines to the end of the catalina.properties:
          org.apache.tomcat.util.buf.UDecoder.ALLOW_ENCODED_SLASH=true
          
After adding the line Screenshot:

![add](https://user-images.githubusercontent.com/67967749/224782272-ae35139a-f2b5-4211-a54a-b2404a5d3a69.png)

#### 1.3.5 Adding the roles to TomCat9 users:
      sudo nano /etc/tomcat9/tomcat9-users.xml
      
###### add the following, N/B: Remember to Change the Password attribute to match your machine password:
    <role rolename="tomcat"/>
    <role rolename="manager-gui"/>
    <role rolename="admin-gui"/>
    <user username="admin" password="PASSWORD" roles=" tomcat, manager-gui, admin-gui"/>

After adding the lines and editing the password:

![Screenshot from 2023-03-13 20-39-39](https://user-images.githubusercontent.com/67967749/224784335-4aca7240-ee39-427d-8196-a1ccb57b2bd6.png)

#### 1.3.6 Optimizing TomCat9. N/B uncomment and add Java_home Don't copy the path of JAVA:
     sudo nano /etc/default/tomcat9
     
###### uncomment JAVA_HOME as shown below:
 ![java](https://user-images.githubusercontent.com/67967749/224786618-a6ba1ba8-8381-4d6d-95ed-21662b64a5ca.png)
 
###### on the same nano /etc/default/tomcat9 add this line:
      JAVA_OPTS="${JAVA_OPTS} -Xmx2048m -Xms1024m -XX:PermSize=512m -XX:MaxPermSize=512m -XX:NewSize=256m"
      
###### After the line to the nano /etc/default/tomcat9 as shown:

![addittion](https://user-images.githubusercontent.com/67967749/224788682-38fed7ce-e2bc-46e7-84fc-9ed24369b206.png)

###### N/B: Ensure your machine has 8GB RAM or Edit the JAVA_OPTS line to fit your RAM size 

#### 1.3.6 Create OpenMRS Directory and grant User Permission to TomCat9
      sudo mkdir /var/lib/OpenMRS 
      sudo chown -R tomcat:tomcat /var/lib/OpenMRS/
      sudo chmod -R 755 /var/lib/OpenMRS*
      
###### Creating the OpenMRS

![mkdirOpenMrs](https://user-images.githubusercontent.com/67967749/224792116-1346482c-d679-41b0-bc2e-61f20afc812a.png)

######  "sudo chown -R tomcat:tomcat /var/lib/OpenMRS/" changes the owner and group of the "/var/lib/OpenMRS/" directory and its contents to "tomcat" user and group.

######  "sudo chmod -R 755 /var/lib/OpenMRS*" sets the file permissions of all files and directories that start with "/var/lib/OpenMRS" to 755 recursively

as shown below:
![change](https://user-images.githubusercontent.com/67967749/224791939-a6274cf6-a43c-495a-82c5-728a0800757d.png)



#### 1.3.7 Redirecting Logs to Catalina:
    sudo nano /lib/systemd/system/tomcat9.service
    
###### comment out line
    - SyslogIdentifier=tomcat9
###### add this lines below SyslogIdentifier=tomcat9 commented
      StandardOutput=append:/var/log/tomcat9/catalina.out
      StandardError=append:/var/log/tomcat9/catalina.out
      
as shown:

![catalina](https://user-images.githubusercontent.com/67967749/224795164-328a38a6-ee05-4168-bf07-448597afff02.png)

###### On tomcat9.service file still add this line under Service
      ReadWritePaths=/var/lib/OpenMRS/
   
at end you should have this:

![datalina](https://user-images.githubusercontent.com/67967749/224796082-b7abe63c-e2da-4c0b-b940-bcc5dd958d5e.png)

#### 1.3.8 Creating Service.d:
    sudo mkdir /etc/systemd/system/tomcat9.service.d
    
#### 1.3.9 Create blank file and name it logging-allow.conf:
     sudo nano /etc/systemd/system/tomcat9.service.d/logging-allow.conf
     
as show below:

![loggi](https://user-images.githubusercontent.com/67967749/224798190-60d701f8-1c17-47b3-b656-37aabb25166e.png)

###### Once Created, add the below lines and save the changes:
       ReadWritePaths=/var/lib/OpenMRS
       
as shown:

![config](https://user-images.githubusercontent.com/67967749/224798678-1984d6d1-15f7-44f8-82d0-8ed71b54b84b.png)

#### 1.3.9 Reload the daemon process and restart tomcat9:
      sudo systemctl daemon-reload
      sudo systemctl restart tomcat9
