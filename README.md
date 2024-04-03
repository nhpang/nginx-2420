**Hello!**

Welcome to Nathan (Pang)'s guide to Nginx.
This is a full guide to installing and running nginx servers on an ArchLinux computer. Before installing or configuring any file, make sure your Linux isup to date. You can update your linux using the following command:

	`sudo pacman -Syu`

Once your Linux is up to date, we can start by installing all our necessary softwares.


**Installing Vim**

In order to create servers with nginx, one must need to alter configuration files, which requires Vim. Vim is a text editing software commonly used on Linux machines. It In order to install Vim, run the following command:

	`sudo pacman -S vim`

This code will install Vim for you. Now that we can edit files, we can install nginx.


**Installing Nginx**

Installing nginx is as simple as installing Vim. The following command will install nginx:

	`sudo pacman -S nginx`

Now that nginx is installed, let's try and see if it works. Use the following command to start an nginx server:

	`sudo systemctl start nginx`

The *systemctl* command stands for system control, which by definition, is acommand that allows users to control and manage the Linux system. By calling *sudo systemctl start*, we are telling Linux to start a process on our system. Of course that process was the next argument, which is *nginx.*

Now, our nginx should be active. In order to check, use the following line of code:

	`sudo systemctl status nginx`

Similar to our last command, this command tells Linux to check the status ofactive processes, this one being nginx.

This command should return a list of information regarding nginx, but the
one we are interested in is *Active*. This will, of course, tell us if nginxis currently active. If *Active* returns *active* in green, that means nginx is active and everything is good. However, if *Active* returns *failed* in red, nginx is not running. Please ensure the start code does not have typos.

In order to stop nginx from running, run the following code:

	`sudo systemctl stop nginx`


**Nginx Configuration**

Now that nginx can properly run, we can configure our server to host our owndata.

To change where our server is hosted and what is hosted, we have to change the configuration files. Configuration files are found in where all other system configuartion files are: */etc*. In */etc*, we can find a directory called nginx, which is where all our nginx configurations are found. Use thefollowing code to begin changing our nginx configurations:

	`sudo vim /etc/nginx/nginx.conf`

After using this command, you will find yourself in a long text file with all of nginx's configurations.

Before we change any nginx configurations, it is important to understand how this configuration file works. Upon opening *nginx.conf*, you will find two blocks:

```
	events {
	}

	http {
	}
```

Of course, these blocks will be filled with information. We can ignore the *events* block for now, as the *http* block will have all our desired configurations. The *http* block will hold all our servers that run with http, and of course, each server is hosted with it's own *server* block, like the
following code:

```
	server {
		listen
		server_name
	
		location \ {
			root
			index
		}
	}
```

**Nginx Server Blocks**

These server blocks will hold all information regarding each respective server. In order to understand the server blocks, it is important to define each line.

listen - this will determine what port your server is held in  
server_name - this is where your server is held. Domain names often go here. If you would like to use your IP address, use an underscore (_)

root - this is the directory of what your server will hold  
index - this is for files in the directory that will be hosted. HTML and HTM files often go here


For now, let's try hosting our own server. Under the *http* block, I will have a single server block. The server looks as so:

```
	server {
		listen 80;
		server_name _;

		location \ {
			root /file/location/directory
			index website.html
		}
	}
```

This server will be hosted on port 80 (http) and the browser address will be the user's IP address, as an underscore is used. For the files, the file is located in */file/location/directory* and the file itself is named *website.html*.

There we go! Open a browser and type in your IP address. You should be able to find a website with your HTML.


**Your server is now hosted with nginx. Happy hosting!**
Nathan Pang
