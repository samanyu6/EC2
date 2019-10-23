## App Deployment

### 1. EC2 Install Scripts: 

```sudo apt-get update ```

``` sudo apt-get install python3-pip```

```pip3 install flask```

 ``` sudo apt-get install nginx```
 
 ```  sudo apt-get install gunicorn3```
 
 ``` pip3 install flask_mail ```
 
 ``` pip3 install boto3 ```
 
 ``` pip3 install Flask-Scss	```
 
 ```pip3 install awscli --upgrade --user```
 
 ``` aws configure ```
 
 - ``` Enter aws credentials from accesskey file```
 ``` Look up aws region codes based on your location, refer this link https://docs.aws.amazon.com/general/latest/gr/rande.html```
 
 ```mkdir ~/flaskapp```
 
 - ```-- Push flask files in flaskapp via fileZilla or any other application ```

### Nginx Setup 
 ``` cd /etc/nginx/sites-enabled/ ```
 
```sudo vi flaskapp```

``` 
server{
   listen 80;
   server_name 3.14.134.98;

   location / {
      proxy_pass http://127.0.0.1:8000;
    include proxy_params;
    
   }
}
 ```
 
 ```sudo service nginx restart ```


### Gunicorn 

``` gunicorn3 app:app```

### Edit Security Options on EC2

``` Add HTTP and HTTPS in security group on ports 80 and 443 and 8080```

### Gunicorn as a service

```cd /etc/systemd/system/```

```sudo vim  gunicorn3.service```
``` 
[Unit]
Description=Gunicorn service
After= network.target

[Service]
User = ubuntu
Group = www-data
WorkingDirectory = /home/ubuntu/flaskapp
ExecStart = /usr/bin/gunicorn3 --workers 3 --bind unix:flaskapp.sock -m 007 app:app

```

```sudo systemctl daemon-reload ```

``` sudo service gunicorn3 start```

``` sudo service gunicorn3 status ```

```cd /etc/nginx/sites-enabled/```

- add: 
``` proxy_pass http://unix:/home/ubuntu/flaskapp/flaskapp.sock ```

``` sudo service nginx restart ```

``` sudo service gunicorn3 restart ```

### Set up elastic IP on aws

- Open elastic IP option
- Select amazon pool option
- Go back to elastic IP options on the side under Network and Security 
- Select Action and connect instance to the elastic IP

### If instance is terminated 
- run the following commands to restart server 
- ``` sudo service nginx start```
- ``` sudo service gunicorn3 start```
