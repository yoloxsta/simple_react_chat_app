# ✨️ Deploying a fullstack React Application on AWS EC2


### Set up an AWS EC2 instance

1. Create an EC2 instance
    - Select an OS image - Ubuntu
    - Create a new key pair & download `.pem` file
    - Instance type - t2.micro
2. Connecting to the instance using ssh
```
ssh -i instance.pem ubunutu@<IP_ADDRESS>
```

### Configuring Ubuntu on remote VM

1. Updating the outdated packages and dependencies
```
sudo apt update
```
2. Install nginx for high-performance web serving, efficient load balancing, and secure SSL/TLS termination.
```
sudo apt install nginx -y
```
3. Install nodejs it is required to run the server-side code of your application.
```
sudo apt install nodejs -y
```
4. Install nodejs npm(Node Package Manager). npm is used to manage and install the dependencies of your Node.js application, including those required for your React front-end.
```
sudo apt install nodejs npm
```
5. Install pm2 it is a process manager for Node.js applications
```
sudo npm install pm2@latest -g
```


### Deploying the project on AWS

1. Clone this project in the remote VM
```
git clone https://github.com/VisheshGhule/simple_react_chat_app.git
```
2. Install npm first in the client directory and then in the server directory
```
npm install
```
3. Start Application
```
pm2 start app.js --name <chat_app>
```
4. Do pm2 logs 0 it helps identify issues & debug errors.
```
pm2 logs 0
```
5. If you get any error then resolve it by typing command
``` npm i express ``` after that restart pm2 by typing command ``` pm2 restart all ``` then again do ``` pm2 logs 0 ```
6. Edit Nginx Configuration by typing command - `sudo vim /etc/nginx/sites-available/default` 
```
server {
    listen 80 default_server;
    listen [::]:80 default_server;


#root /var/www/html;

# Add index.php to the list if you are using PHP
#index index.html index.htm index.nginx-debian.html;

    server_name _;

    location / {
        proxy_pass http://localhost:5001;
                proxy_http_version 1.1;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection ‘upgrade';
                proxy_set_header Host $host;
                proxy_cache_bypass $http_upgrade;
}
```
7. Do **sudo nginx -t**, this command is used to test the Nginx configuration for syntax errors before applying changes.
```
sudo nginx -t
```
8. Restart nginx
```
sudo service nginx restart 
```

> NOTE - We will have to edit the **inbound rules** in the security group of our EC2, in order to allow traffic from our particular port.

### Project is deployed on AWS 🎉

---



## ✨️ To understand it better & in depth, please visit the blog provided below
<a href="https://visheshblog.hashnode.dev/day-30-deploying-fullstack-react-application-on-aws-ec2">Blog Link</a>
