**Hello!**

Welcome to Nathan (Pang)'s guide to firewalls and backends on Archlinux.

Firewalls are meant to protect your computer by limiting data that goes into your computer while allowing data to be sent from your computer.

**Updating Linux**

Before doing anything, we must first ensure our Linux distribution is up to date. To update Archlinux, use the following command:

    sudo pacman -Syu

Using *-Syu* will also update your kernel. To ensure kernel updates are applied, reboot your Linux. Use the following command to do so:

    sudo reboot

**Installing a Firewall**

The firewall we will be using is ufw. Ufw stands for Uncomplicated Firewall. To install ufw, use the following command:

    sudo pacman -S ufw

We can use the following code to check the status of our ufw:

    sudo ufw status verbose

It should return inactive. We haven't turned it on yet.

**Configuring the Firewall**

Now that we have our firewall, we need to configure what the firewall will actually do. We can choose what our firewall will deny and allow. The following command are common settings, as they allow for a decent amount of protection:

    sudo ufw default deny incoming
    sudo ufw default allow outgoing

In order to limit ssh connections, use the following command:

    sudo ufw limit ssh

We can also allow certain connections, such as http.

    sudo ufw allow http

**Starting the Firewall**

In order to start the firewall, use the following command:

    sudo ufw enable

And once again we can use *status* to check if it is running.

    sudo ufw status verbose

This command should return a table. The three columns are "To", "Action", and "From". Each row will be a connection, which you can see where it's sent to, what it is doing, and where it is from.


**Creating a Reverse Proxy Server**

If you have an nginx server, it might be best to ensure it is secure from dangers. Setting up a reverse proxy server is always a good idea, which can be done with nginx.

We can first start by having our backend binary. For this example, I will be using *hello-server* as my backend binary.

We must first move the backend binary to a new directory, and since we are using nginx we will move it like so:

    sudo mv hello-server /etc/nginx

**Creating a Service File**

We will first need to make a service file for our backend binary. My service file will be called *hello-server.service*. This file must be in our */systemd/system* directory.

    sudo touch hello-server.service
    sudo mv hello-server.service /etc/systemd/system/

The service file will have the following code:

    [Unit]
    Description=Nathan's Backend Application Example

    [Service]
    Type=simple
    ExecStart=/etc/nginx/hello-server

    [Install]
    WantedBy=multi-user.target

If your backend binary is in a different location, make sure that *ExecStart* links to the correct location.


**Configuring a Reverse Proxy**

We can create a new configuration file for the reverse proxy. The configuration file must also be in */etc/nginx/*. In this example, *server.conf* will have the following code: 

    server {
        listen 80;
        server_name 24.199.113.152;

        location /hey {
            proxy_pass http://127.0.0.1:8080/hey;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }

        location /echo {
            proxy_pass http://127.0.0.1:8080/echo;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
    }

Now, we must reload services using:

    sudo systemctl daemon-reload

We can start the backend using:

    sudo systemctl start hello-server.service

Finally, we can restart nginx using:

    sudo systemctl restart nginx
