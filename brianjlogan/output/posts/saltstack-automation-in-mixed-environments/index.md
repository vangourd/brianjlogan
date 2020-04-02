.. title: SaltStack Automation in Mixed Environments
.. slug: saltstack-automation-in-mixed-environments
.. date: 2020-02-12 14:12:23 UTC-05:00
.. tags: python,saltstack,cisco,procurve
.. category: automation
.. link: 
.. description:  My findings on saltstack network automation. 
.. type: text

<video width="360" height="280" autoplay loop>
    <source src="/saltstack.mp4" type="video/mp4">
    Your browser does not support the video tag
</video>

My goal is for this article to give someone a jump start on system automation in smaller mixed environments. You are likely a network engineer or sysadmin who has some experience in the field and has heard a lot about Python or Powershell automation. 

SaltStack is a wonderful framework to get you automating without having to solve the difficult problems with distributed computing. 

<!-- TEASER_END -->

# Learn Systems Automation

Systems Automation frameworks combine powerful tools for querying, modifying, reacting to, and ultimately mastering the state of your infrastructure. Without these tools you will be forced to mimic their utilites using scripting or programming languages. Make your life easier! 

SaltStack provides open source modular components for maximum interoperability with the systems it must manage. It does certain jobs well and leaves the rest up to SysAdmins to develop and implement for their environment. It has uses cases for administrating desktops, servers, and networking hardware. It has a place in the cloud, at home, and in the datacenter. All the while you're enjoying the benefits of abstracting and automating your work away you will be capturing valuable insights into the configuration model documenting it for all to understand. 

# Automate your Desktop Environment

Most workstation configurations are performed by Desktop support and scripted when possible. Set your system to auto-configure based on the metadata you assign to a machine. If there are differences between *Marketing* and *Accounting* then reduce your configuration step from performing the differences yourself to assigning the metadata and allowing the automation system to take it from there.  

- Deploy software from your package manager
  - Lock versions for compatibility
  - Push updates for known security patches
- Push Audit logging rule granularity based on machine risk
- Tighten Firewalls after receiving IPS Events
- Roll out changes, test, and roll-back with poor results

# Automate your Servers and your Services

Services transcend individual servers. Define a state for your service and deploy it to all of the servers serving it. Define the firewall exceptions, the software requirements, the scheduled tasks, even the resource requirements and deploy. Understand the consistencies in your infrastructure and make compatible changes throughout your environment.

# Automate your network

Version control your network configuration. Capture "snowflakiness" with conditionals and metadata to be monitored and evolved. Transcend single manufacturer deployments by abstracting the differences.  

**Choose SaltStack over Puppet, Ansible, Chef, or others**

It's mature enough to be used in the enterprise.

It's written in Python3 which means reading and understanding the internals when someone didn't have time to write documentation is feasible. 

It's a very easy to use framework for custom automation. It doesn't do EVERYTHING out of the box (even though it feels like it at first) but with Python3 it's a script, or an [execution module](https://docs.saltstack.com/en/latest/ref/modules/index.html), a [runner](https://docs.saltstack.com/en/latest/ref/runners/),a [returner](https://docs.saltstack.com/en/latest/ref/returners/), or a [proxy module](https://docs.saltstack.com/en/master/ref/proxy/all/index.html) away. You get the idea 💡!

It's open source and free to use.

It is extremely easy to get started on both Windows, Mac, or Linux.

Jinja2 and the Python specificaton are much easier to read and write than DSL's in other languages. Additionally use these skills for anything else Python related. 
