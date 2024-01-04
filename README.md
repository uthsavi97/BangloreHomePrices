
I developed a web application that harnesses the power of machine learning to provide accurate and insightful real estate price predictions in Bangalore, India.
# Key Project Features:
# Data-Driven Insights:
- Rigorously collected and cleaned extensive real estate data to uncover meaningful patterns.
- Employed sophisticated data visualization techniques to communicate insights effectively. 
# Intelligent Price Prediction:
- Built a robust machine learning model using linear regression to predict property values with a high degree of accuracy.
- Optimized model performance through meticulous hyperparameter tuning and cross-validation techniques.
# User-Friendly Web Interface:
- Designed an intuitive and visually appealing web application using Flask and front-end technologies.
- Enabled users to seamlessly input property details and receive instant price estimates. 
# Technical Skills Showcased:
- Python
- NumPy and Pandas for data manipulation
- Matplotlib and Seaborn for data visualization
- Scikit-learn for machine learning modeling
- Flask for web development
- HTML/CSS/JavaScript for front-end development

# Deploy this app to cloud (AWS EC2)

1. Create EC2 instance using amazon console, also in security group add a rule to allow HTTP incoming traffic
2. Now connect instance using a command like this,
```
ssh -i "C:\Users\karba\.ssh\Banglore.pem" ubuntu@ec2-3-133-88-210.us-east-2.compute.amazonaws.com
```
3. nginx setup
   1. Install nginx on EC2 instance using these commands,
   ```
   sudo apt-get update
   sudo apt-get install nginx
   ```
   2. Above will install nginx as well as run it. Check status of nginx using
   ```
   sudo service nginx status
   ```
   3. Here are the commands to start/stop/restart nginx
   ```
   sudo service nginx start
   sudo service nginx stop
   sudo service nginx restart
   ```
   4. Now when load cloud url in browser will see a message saying "welcome to nginx" This means your nginx is setup and running.
4. Now need to copy all code to EC2 instance. Either using git or copy files using winscp. We will use winscp.Download winscp from here: https://winscp.net/eng/download.php
5. Once connect to EC2 instance from winscp,now copy all code files into /home/ubuntu/ folder. The full path of root folder is now: **/home/ubuntu/BangloreHomePrices**
6.  After copying code on EC2 server now we can point nginx to load our property website by default. For below steps,
    1. Create this file /etc/nginx/sites-available/bhp.conf. The file content looks like this,
    ```
    server {
	    listen 80;
            server_name bhp;
            root /home/ubuntu/BangloreHomePrices/client;
            index app.html;
            location /api/ {
                 rewrite ^/api(.*) $1 break;
                 proxy_pass http://127.0.0.1:5000;
            }
    }
    ```
    2. Create symlink for this file in /etc/nginx/sites-enabled by running this command,
    ```
    sudo ln -v -s /etc/nginx/sites-available/bhp.conf
    ```
    3. Remove symlink for default file in /etc/nginx/sites-enabled directory,
    ```
    sudo unlink default
    ```
    4. Restart nginx,
    ```
    sudo service nginx restart
    ```
7. Now install python packages and start flask server
```
sudo apt-get install python3-pip
sudo pip3 install -r /home/ubuntu/BangloreHomePrices/server/requirements.txt
python3 /home/ubuntu/BangloreHomePrices/client/server.py
```
Running last command above will prompt that server is running on port 5000.
8. Now just load cloud url in browser (for me it was http://ec2-3-133-88-210.us-east-2.compute.amazonaws.com/) and this will be fully functional website running in production cloud environment
