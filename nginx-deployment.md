# Deploying Your Portfolio Website on Nginx

This guide will walk you through the steps to deploy your portfolio website on Nginx, using `portfolio` as the website directory and `portfolio.conf` as the configuration file. The domain for the website will be set as `myportfolio.com`.

## Prerequisites
- Ubuntu operating system
- Basic knowledge of terminal commands
- Nginx installed on your system
- Your website files in a directory named `portfolio`

## Steps

### Step 1: Install Nginx
First, ensure that Nginx is installed on your Ubuntu system. If it's not already installed, you can install it using the following commands:

```sh
sudo apt update
sudo apt install nginx
```

### Step 2: Create the Website Directory
Assuming your website directory is named `portfolio` and is located in your home directory, copy it to the Nginx web directory:

```sh
sudo cp -r ~/portfolio /var/www/html/
```

### Step 3: Create the Nginx Configuration File
Next, create a new configuration file for your website in the Nginx sites-available directory:

```sh
sudo vi /etc/nginx/sites-available/portfolio.conf
```

Add the following content to the file:

```nginx
server {
    listen 80;
    server_name myportfolio.com www.myportfolio.com;

    root /var/www/html/portfolio;
    index index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }

    error_log /var/log/nginx/portfolio_error.log;
    access_log /var/log/nginx/portfolio_access.log;
}
```

This configuration file sets up a server block for your website, specifying the document root and log file locations.

### Step 4: Enable the Configuration
Enable the new site configuration by creating a symbolic link in the sites-enabled directory:

```sh
sudo ln -s /etc/nginx/sites-available/portfolio.conf /etc/nginx/sites-enabled/
```

### Step 5: Test Nginx Configuration
Test the Nginx configuration for syntax errors:

```sh
sudo nginx -t
```

If the test is successful, you will see a message indicating that the configuration is OK.

### Step 6: Restart Nginx
Restart Nginx to apply the changes:

```sh
sudo systemctl restart nginx
```

### Step 7: Update Your Hosts File (For Local Testing)
If you are testing locally and don't have a domain name yet, you can update your `/etc/hosts` file to map `myportfolio.com` to `localhost`:

```sh
sudo vi /etc/hosts
```

Add the following line:

```sh
127.0.0.1   myportfolio.com
```

### Step 8: DNS Configuration (For Live Domain)
If you have a domain name, update the DNS settings of your domain registrar to point to your server's IP address. This typically involves setting an A record for `myportfolio.com` and `www.myportfolio.com` to your server's IP.

### Step 9: Test Your Website
Finally, open a web browser and navigate to `http://myportfolio.com` to verify that your website is up and running.

## Conclusion
By following these steps, you should have your portfolio website successfully deployed on Nginx with the domain `myportfolio.com`. If you encounter any issues, make sure to check the Nginx error logs for more information:

```sh
sudo tail -f /var/log/nginx/portfolio_error.log
```
```


