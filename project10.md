## Documentation of Project 10

**Summary**

- If you have been following keenly the documentation from project 7, you would understand that all the seemingly bugging commands we have been deploying is targeted at one particulat endpoint. Starting from the deployment of the two Webservers, NFS, Database and the additonal Jenkins Project, all done with the aim of ensuring our already publised Tooling Website is strong and worthy of standing the test of time. In the previous projects, we have published a functional website accessible through the two webservers, deployed load balancer solution with Apache/Httpd, ensured there a Continuous Integration Pipeline for the tooling website, now we will now proceed to load balancer solution with Nginx And SSL/TLS. At the end of this project, in addition to what we have previously deployed, the image below is our target architecture. ![THE TARGET ARCHITECTURE](https://github.com/Lordchancellorr/project-10/blob/main/Images/Final%20Architecture.PNG)
- In this project, what you should expect is the seamless deployment of Load Balancer Solution using Nginx with SSL/TLS. However, you might be wondering why we need to go over the whole process again in different dimension. Truth is, as an aspiring  DevOps engineer, you are being brooded to accept the fact that 5+5 is not the only combination of numbers that can give us 10, 1+9,2+8, 3+7 will equally give us 10! what happens when 5+5 fails to give 10? Are we just gonna wait and fold hands? Definitely not! That is why knowing how to deploy load balancer solution with Nginx is very essential and it will work just fine like the ones we did with Apache and that is exactly what we are about to do. Additionally, for the very first time, you'd learn how to generate a domain name for free and how to secure?protect it accordingly to prevent external attack from cybercriminals using [SSL](https://en.wikipedia.org/wiki/Secure_Sockets_Layer) and [TSL](https://en.wikipedia.org/wiki/Secure_Sockets_Layer) - (Its newer version).  

In your free time, read more about the SSL certificate [here](https://blog.hubspot.com/marketing/what-is-ssl) and do well to watch the videos on how it works [here](https://youtu.be/T4Df5_cojAs). For further study, click [here](https://youtu.be/SJJmoDZ3il8)

**Implementation of Project 10**

- Create an EC2 VM based on Ubuntu Server 20.04 LTS and name it Nginx LB and do not forget to open TCP port 80 for HTTP connections, also open TCP port 443 – this port is used for secured HTTPS connections)
- Update `/etc/hosts` file for local DNS with Web Servers’ names and their local IP addresses
- After launching your Nginx instance, connect your virtual server on the terminal and then run `sudo apt update && sudo apt install nginx` to update and install nginx.
- Before proceeding to the next step, let's quickly generate a domain for free using [freenom](https://my.freenom.com/clientarea.php). register on  [freenom](https://my.freenom.com/clientarea.php)and click on **Services,** generate a new domain, if it is available, place your order and you will be notified that your domain is now active. Check the image below: ![Domain](https://github.com/Lordchancellorr/project-10/blob/main/Images/My%20domain.PNG) If the above page displays, that means your domain is active and running. You can confirm the activeness by pasting it on a web browser.
- There is need to configure the Nginx webserver with our new domain name. Follow the following steps:
- Go to your AWS acount, search for Route 53 then click on it.
- Create a hosted zone
- Go back to your domain, copy and paste it on the created hosted zone, then create
- In other for the route 53 to be connected to your domain,copy your name servers from the route 53 to the free domain. On your free domain account(the image above), click on **MANAGEMENT TOOLS** then click on use custom nameservers. 
- Copy each of the nameservers in your route 53(check the image below) ![Route 53](https://github.com/Lordchancellorr/project-10/blob/main/Images/Route%2053%20hosted%20zone.PNG) then click on change nameservers.This simply means your domain nname and route 53 a are now connected.
- Copy the Nginx public IP,go back to your route 53 and click on **create a record.** Leave everything as default but paste the Nginx IP address you copied inside a box tagged **VALUE** then click on *create records.* Create another record 
, leave everything as default but add inside a box tagged as **RECORD NAME,** type in "www" then paste the IP address inside **VALUE** as done previously, then create record. Now you have sucesssfully connected your domain to the nginx webserver in way that if you try to browse your domain name on any web browser or device, it will display: **WELCOME TO NGINX.**

- Go back to your terminal, run ` sudo systemctl enable nginx &&  sudo systemctl start nginx` to enable Nginx. Run `sudo systemctl status nginx` to confirm the status. The image below should be your output- ![Nginx status](https://github.com/Lordchancellorr/project-10/blob/main/Images/Nginx%20status.PNG) 

- Create a configuration for your reverse proxy setting. Run- `sudo nano /etc/nginx/sites-available/load_balancer.conf` and paste your web server information, please do well to edit and provide the relevant information like the server and domain name, control O to save the file, enter and Control x to exit.

- In other for the new reverse proxy to be redirected to the enabled sites, remove the default sites with this command- `sudo rm -f /etc/nginx/sites-enabled/default` To know if your nginx was succesfully configured, run `nginx -t`, the image should be your output: ![Nginx Configuration](https://github.com/Lordchancellorr/project-10/blob/main/Images/Successful%20nginx%20configuration.PNG)

Change directory to ` /etc/nginx/sites-enabled` and link your load balancer configuration file to youe sites availabe to that nginx can access it seamlessly. Use this command `sudo ln -s ../sites-available/load_balancer.conf .` Run ls and the image below should be your output. ![Successful Link](https://github.com/Lordchancellorr/project-10/blob/main/Images/linking%20the%20load%20balancer%20conf.PNG)
Then reload your nginx with `sudo systemctl reload nginx`

If you visit the domain name on any web browser, after a successful login(provided you are still connected to your database), the image below should be your output. ![Propitix webpage](https://github.com/Lordchancellorr/project-10/blob/main/Images/propitix%20webpage%20accessed%20with%20domain%20name.PNG)
Did you observe it says the connection is **NOT SECURE**? YEAH! That's what we are about to do! Let's go back to our terminal!

Run the following commands: `sudo apt install python3-certbot-nginx -y` see the image below: ![python](https://github.com/Lordchancellorr/project-10/blob/main/Images/Python%203%20installation.PNG) Check your syntax and reload the nginx. Let's create a certificate for your domain. Run- ` sudo certbot --nginx -d myproject10.ga -d www.myproject10.ga` (make sure you edit where applicable. i.e your domain), enter a valid email address, read the service agreement, press A to agree, if you are willing to share your certificate with the community, type yes, then wait while your certificate is generated. You can grab a cup water(lol) while you wait. type 2 to redirect all incoming request from port 80 to port 443. If you folowed the instruction closely, the image below should be your output. ![certification](https://github.com/Lordchancellorr/project-10/blob/main/Images/Certification.PNG)
Congratulations for a job welldone! Now go back to your web browser and reload the domain page. Holla! Your connection should now be secured! See the image below- ![Secured Connection](https://github.com/Lordchancellorr/project-10/blob/main/Images/secured%20connection.PNG)
If the above image is your result, a hearty congratulations to you for a job well done. Thanks for your patience.

Do have a nice day!
