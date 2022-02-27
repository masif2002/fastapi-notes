# NGINX

NGINX is an intermediary webserver that acts as a proxy. 
* It receieves the original request and then proxies it to the appliction
* It can handle SSL termination, which means that when a HTTPS request is sent to the NGINX server, it processes it and then forwards it as a plain HTTP packet to our application.

## Setting up NGINX

* Install NGINX
    ```
    sudo apt install nginx
    ```
* Startup the NGINX service
    ```
    systemctl start nginx
    ```
* Configure NGINX
    * There is a config file named _default_ in `/etc/nginx/sites-available/`. It cointains the default configuration for nginx. 
    * The only thing we need to change is the _location_ block in the config file similar to the one [here](https://github.com/masif2002/fastapi/blob/master/nginx).
    * Restarting the _nginx_ service should display the changes.

## SSL certification
You can head onto [certbot.eff.org](https://certbot.eff.org/) and follow the instructions to get an SSL certificate for your website.