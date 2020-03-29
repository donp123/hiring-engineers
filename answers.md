# The following will discuss the steps necessary in order to complete the Datadog Sales Engineering Lab.

## Prerequisites - Setup the environment
In order to properly complete the Datadog Sales Engineering Lab I first had to complete the environment setup. I choose to spin up a fresh new linux VM using Vagrant. In order to download and install a VM with Vagrant I followed the instructions provided in the reference section of the Datadog Sales Engineering lab by first downloading VirtualBox, and then downloading the latest version of Vagrant(2.2.7). 

From here I ran the following two commands in order to have a fully running virtual machine in Virtual box:

![Hostmap](https://github.com/donp123/donp123/blob/master/vagrantup.png)

I then signed up for a trial with Datadog using  “Datadog Recruiting Candidate” in the “Company” field, and proceeded to download the appropriate Agent for linux. I used the following command for the Datadog 7 installation onto my VM:

DD_AGENT_MAJOR_VERSION=7 DD_API_KEY=f622b4c53cca8fc2fd7e0f74c02e302b bash -c "$(curl -L https://raw.githubusercontent.com/DataDog/datadog-agent/master/cmd/agent/install_script.sh)"



## Collecting Metrics 

Below is the picture of my hostmap:
![Hostmap](https://github.com/donp123/donp123/blob/master/hostmap.png)



## Visualizing Data


## Monitoring Data


## Collecting APM Data
Given the following Flask app (or any Python/Ruby/Go app of your choice) instrument this using Datadog’s APM solution:

picture of APM code

## Final Question






