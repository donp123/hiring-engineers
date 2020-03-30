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

### 1.) Add tags in the Agent config file and show us a screenshot of your host and its tags on the Host Map page in Datadog.

  In order to complete this task, I first had to edit the datadog.yaml by running the following command:
  
  $ sudo vi /etc/datadog-agent/datadog.yaml

  Once inside the file file I was able to add in tags for my agent:
  
 ![tags](https://github.com/donp123/donp123/blob/master/datadogyaml.png)
  
  I started off by adding one tag "env:dev" for my agent to make sure it would work. I then saved the  yaml file and restarted the
  agent: $ sudo service datadog-agent restart
  
  Once restarted, I went back into Datadog to check my Host Map:

![Hostmap](https://github.com/donp123/donp123/blob/master/hostmap.png)

  I then saw that my tag has been succesfully added. At this point I went back and added a second tag "tier:webserver".
  
### 2.) Install a database on your machine (MongoDB, MySQL, or PostgreSQL) and then install the respective Datadog integration for that database.

  In order to complete this step I first chose to install MySQL onto my VM by running the following commands:
    $ sudo apt update
    $ sudo apt install mysql-server
    $ sudo mysql_secure_installation
    
    
  I then had to install the Datadog integration for the MySQL database, which I found here:    https://docs.datadoghq.com/integrations/mysql/
  
  The first step given for the Datadog integration was to create a new database user called datadog:
  mysql> CREATE USER 'datadog'@'localhost' IDENTIFIED BY 'password';
  
  I then granted the user proper permissions:
  
  mysql> GRANT REPLICATION CLIENT ON *.* TO 'datadog'@'localhost' WITH MAX_USER_CONNECTIONS 5;
  Query OK, 0 rows affected, 1 warning (0.00 sec)

  mysql> GRANT PROCESS ON *.* TO 'datadog'@'localhost';
  Query OK, 0 rows affected (0.00 sec)
  
  
  I then had to add the proper configuration block to my conf.yaml file to collect the MySQL metrics:
 
   $ vi /etc/datadog-agent/conf.d/mysql.d/conf.yaml
 
  From here I added the following code to the file:
  
  ![yaml](https://github.com/donp123/donp123/blob/master/confyam.png)
  
  I then restarted the Datadog agent:
  
  $ systemct1 restart datadog-agent
  
  
  I now went into the Datadog UI and saw that a new dashboard was added called "MySQL Overview":
  
  ![mysqldash](https://github.com/donp123/donp123/blob/master/mysql.png)
  
  This dashbaord is showing that my agent is correctly setup and is now collecting from the MySQL databse I installed.
  
### 3.) Create a custom Agent check that submits a metric named my_metric with a random value between 0 and 1000.

  In order to complete this task, I first had to create a metric check python file. I called this file
  my_metric_checks.py:
  
  $ sudo vi /etc/datadog-agent/checks.d/my-metric-check.py
   
  I then added the following code into the python file:
  
  ![metriccheck](https://github.com/donp123/donp123/blob/master/pythonrand.png)
  
  
  I then had to create a check Yaml file that also had the same name has my check Python file:
  
  $ sudo vi /etc/datadog-agent/conf.d/my-metric-check.yaml
  
  I then added the following to this file as directed by the Datadog doc file (https://docs.datadoghq.com/developers/write_agent_check/?tab=agentv6v7) :
  
    instances: [{}]
    
   
  From here I restarted my Datadog-agent and tested to see if my custom metric check was working properly:
  
  ![mymetriccheck](https://github.com/donp123/donp123/blob/master/metriccheck.png)
  
  
### 4.) Change your check's collection interval so that it only submits the metric once every 45 seconds

In order to change my checks collection interval, I first went into the configuration yaml file I created in the   above step and changed the min collectio ninterval to 45. The below figure is taken from  (https://docs.datadoghq.com/developers/write_agent_check/?tab=agentv6v7), where I changed the interval from 30 to  45.

  
![intervalchange](https://github.com/donp123/donp123/blob/master/collectionintchangedoc.png)

![intervalchange2](https://github.com/donp123/donp123/blob/master/collectionchangecode.png)

From here I restarted the Datadog agent, and then went into Datadog to find my custome metric on the "Metric  Explorer":

![mymetricdash](https://github.com/donp123/donp123/blob/master/metricsexplorerinterval.png)

The dashboard confirmed that the colection interval for my metric has been changed to 45 seconds.

### Bonus Question:  Can you change the collection interval without modifying the Python check file you created?

Yes you would just need to go into the configuration file and change the minimum collection interval as we just did to be 45 seconds from 15. There was no change needed to my check python file.

## Visualizing Data

The following were the tasks given for completion of the Visualizing Data section of the lab:

### 1.) Utilize the Datadog API to create a Timeboard that contains:
  - Your custom metric scoped over your host.
  - Any metric from the Integration on your Database with the anomaly function applied.
  - Your custom metric with the rollup function applied to sum up all the points for the past hour into one bucket

I used the Datadog API using curl in order to create a Timeboard called "DPs first Dashboard" with three graphs. The first graph "My_Metric" displays the custom metric I created scoped over my vagrant host. The second graph created displays "MySQL Anomaly performance (CPU)" to show any anomalies in performance for the MySQL database I installed. The third graph being created by the API is to show my metric rolled up and summed over the last hour. The following is the code used to create my timeboard:

![timeboardapi](https://github.com/donp123/donp123/blob/master/timeboardcode.png)

The following is the Timeboard created by the API:

![timeboardapi](https://github.com/donp123/donp123/blob/master/timeboarddash.png)

Link to Timeboard: https://app.datadoghq.com/dashboard/df6-mrj-ffh/dps-first-dashboard?from_ts=1585248711534&to_ts=1585252311534&live=true&tile_size=m

### 2.) Once this is created, access the Dashboard from your Dashboard List in the UI:
  - Set the Timeboard's timeframe to the past 5 minutes
  - Take a snapshot of this graph and use the @ notation to send it to yourself.

I was able to change the timeframe of my Timeboard by using the shortcut "ALT +]" until it was five minutes:

![timeboardcut](https://github.com/donp123/donp123/blob/master/timeboardshortcuts.png)

From this new timeframe I took a snapshot of the "MySQL Anomaly performance (CPU)" graph and sent an email to myself:

![timeboardsnap](https://github.com/donp123/donp123/blob/master/snapshottime.png)

The following is the email I recieved from Datadog:

![timeboardemail](https://github.com/donp123/donp123/blob/master/emailtimeboard.png)

The following is the second email I sent myself:

![timeboardemail2](https://github.com/donp123/donp123/blob/master/secondtimeboardemail.png)


### Bonus Question: What is the Anomaly graph displaying?

The Anomaly graph is showing outliers in red, while the normal expected performance behavior is in the grey shaded area. This essentially using algorithms to predict based on historical data what should be considered normal behavior, and allows a user to see what is 3 points above the standard deviation.


## Monitoring Data

The following were the tasks given for completion of the Monitoring Data section of the lab:

### 1.) Create a new Metric Monitor that watches the average of your custom metric (my_metric) and will alert if it’s above the following values over the past 5 minutes:
  - Warning threshold of 500
  - Alerting threshold of 800
  - And also ensure that it will notify you if there is No Data for this query over the past 10m.
  



## Collecting APM Data
Given the following Flask app (or any Python/Ruby/Go app of your choice) instrument this using Datadog’s APM solution:

picture of APM code

## Final Question






