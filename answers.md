# The following will discuss the steps necessary in order to complete the Datadog Sales Engineering Lab.

## Prerequisites - Setup the environment
In order to properly complete the Datadog Sales Engineering Lab I first had to complete the environment setup. I choose to spin up a fresh new linux VM using Vagrant. In order to download and install a VM with Vagrant I followed the instructions provided in the reference section of the Datadog Sales Engineering lab by first downloading VirtualBox, and then downloading the latest version of Vagrant(2.2.7). 

From here I ran the following two commands in order to have a fully running virtual machine in Virtual box:

![upcommands](https://github.com/donp123/donp123/blob/master/vagrantup.png)

I then ran the command in order to ssh into the VM from my terminal:

$ Vagrant ssh 

From here I then signed up for a trial with Datadog using  “Datadog Recruiting Candidate” in the “Company” field, and proceeded to download the appropriate Agent for linux. I used the following command for the Datadog 7 installation onto my VM:

DD_AGENT_MAJOR_VERSION=7 DD_API_KEY=f622b4c53cca8fc2fd7e0f74c02e302b bash -c "$(curl -L https://raw.githubusercontent.com/DataDog/datadog-agent/master/cmd/agent/install_script.sh)"

Once I ran the above command, I then went into Datadog to see my agent properly connected  by looking at my system dashboard:

![System Dash](https://github.com/donp123/donp123/blob/master/pic1_aftersetup.png)

Now that my agent has been installed correctly, I was able to move on to the following 'Collecting Metrics' section.


## Collecting Metrics 

The following were the tasks given for completion of the Collecting Metrics section of the lab:

### 1.)Add tags in the Agent config file and show us a screenshot of your host and its tags on the Host Map page in Datadog.

  In order to complete this task, I first had to edit the datadog.yaml by running the following command:
  
  $ sudo vi /etc/datadog-agent/datadog.yaml

  Once inside the file file I was able to add in tags for my agent:
  
 ![tags](https://github.com/donp123/donp123/blob/master/datadogyaml.png)
  
  I started off by adding one tag "env:dev" for my agent to make sure it would work. I then saved the  yaml file and restarted the
  agent: $ sudo service datadog-agent restart
  
  Once restarted, I went back into Datadog to check my Host Map:

![Hostmap](https://github.com/donp123/donp123/blob/master/hostmap.png)

  I then saw that my tag has been succesfully added. At this point I went back and added a second tag "tier:webserver".
  
### 2.)Install a database on your machine (MongoDB, MySQL, or PostgreSQL) and then install the respective Datadog integration for that database.

  In order to complete this step I first chose to install MySQL onto my VM by running the following commands:
    $ sudo apt update
    $ sudo apt install mysql-server
    $ sudo mysql_secure_installation
    
    
  I then had to install the Datadog integration for the MySQL database, which I found here:    https://docs.datadoghq.com/integrations/mysql/
  
  The first step given for the Datadog integration was to create a new database user called datadog:
  mysql> CREATE USER 'datadog'@'localhost' IDENTIFIED BY 'password';
  
  

  




## Visualizing Data


## Monitoring Data


## Collecting APM Data
Given the following Flask app (or any Python/Ruby/Go app of your choice) instrument this using Datadog’s APM solution:

picture of APM code

## Final Question






