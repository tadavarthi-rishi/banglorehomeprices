# banglorehomeprices
In this series of data science projects, I will guide you through the step-by-step process of creating a website that predicts real estate prices. We will start by using the sklearn library and linear regression to build a model, utilizing the Bangalore home prices dataset from kaggle.com. Next, we will develop a Python flask server that utilizes the trained model to handle HTTP requests. The third component involves creating a website using HTML, CSS, and JavaScript. This website will allow users to input the square footage area and number of bedrooms for a home, and it will call the Python flask server to retrieve the predicted price.

Throughout the project, I will cover various data science concepts, including data loading and cleaning, outlier detection and removal, feature engineering, dimensionality reduction, hyperparameter tuning using gridsearchcv, and k-fold cross-validation. The project utilizes several technologies and tools, such as Python, Numpy, and Pandas for data cleaning, Matplotlib for data visualization, Sklearn for model building, Jupyter notebook, Visual Studio Code, and PyCharm as IDEs, Python Flask for the HTTP server, and HTML/CSS/JavaScript for the user interface.
![image](https://github.com/tadavarthi-rishi/banglorehomeprices/assets/40834599/c5ea1dca-66e0-4de2-899b-bdc65ef02539)

Deploy this app to cloud (AWS EC2)

Create EC2 instance using amazon console, also in security group add a rule to allow HTTP incoming traffic
Now connect to your instance using a command like this,
ssh -i "C:\Users\Viral\.ssh\Banglore.pem" ubuntu@ec2-3-133-88-210.us-east-2.compute.amazonaws.com
nginx setup
Install nginx on EC2 instance using these commands,
sudo apt-get update
sudo apt-get install nginx
Above will install nginx as well as run it. Check status of nginx using
sudo service nginx status
Here are the commands to start/stop/restart nginx
sudo service nginx start
sudo service nginx stop
sudo service nginx restart

Now when you load cloud url in browser you will see a message saying "welcome to nginx" This means your nginx is setup and running.

Now you need to copy all your code to EC2 instance. You can do this either using git or copy files using winscp. We will use winscp. You can download winscp from here: https://winscp.net/eng/download.php

Once you connect to EC2 instance from winscp (instruction in a youtube video), you can now copy all code files into /home/ubuntu/ folder. The full path of your root folder is now: /home/ubuntu/BangloreHomePrices

After copying code on EC2 server now we can point nginx to load our property website by default. For below steps,

Create this file /etc/nginx/sites-available/bhp.conf. The file content looks like this,

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

Create symlink for this file in /etc/nginx/sites-enabled by running this command,
sudo ln -v -s /etc/nginx/sites-available/bhp.conf

Remove symlink for default file in /etc/nginx/sites-enabled directory,
sudo unlink default
Restart nginx,
sudo service nginx restart

Now install python packages and start flask server
sudo apt-get install python3-pip
sudo pip3 install -r /home/ubuntu/BangloreHomePrices/server/requirements.txt
python3 /home/ubuntu/BangloreHomePrices/client/server.py

Running last command above will prompt that server is running on port 5000. 8. Now just load your cloud url in browser (for me it was http://ec2-3-133-88-210.us-east-2.compute.amazonaws.com/) and this will be fully functional website running in production cloud environment
