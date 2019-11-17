---
Date: 11/1/19
Maintainer: Sami Ahmed
Title: SSH Investigation
---

# My Use Case

Sometimes you need a ton of computing resources, or very specialized computing resources, and your laptop doesn't meet the requirements. (Lesson learned gridsearch).

# Purpose
- Run Jupyter Notebook on your virtual machine VM (EC2)
- Install Anacondas on your VM
- Use SSH to establish a  terminal session in an EC2 (so you can run computationally laborious tasks in the cloud)

# Setup remote shell session
1. Login to your AWS instance (in browser)
2. Select the box next to your running instance
3. Hit the connect button next to lauch instance copy down the text that looks like this:

`ssh -i "Sami_AWS_Key_Metis.pem" ubuntu@ec2-18-217-227-199.us-east-2.compute.amazonaws.com`

Copy and paste into text editor, change it to:

`ssh -i ~/.ssh/Sami_AWS_Key_Metis.pem ubuntu@ec2-18-217-227-199.us-east-2.compute.amazonaws.com`

4. Open up a terminal, logged into your directory for chi19_ds12 (forked repo)
5. Paste edited text, you should get output back starting with:

`Welcome to Ubuntu 18.04.3 LTS (GNU/Linux 4.15.0-1051-aws x86_64)`

 This will dump you into your ubuntu user on the vm. Now we need to install things that enable jupyter to run on this vm.

# Setup Anacondas in your Linux VM
1. Within your ubuntu user run:
`wget https://repo.anaconda.com/archive/Anaconda3-2019.10-Linux-x86_64.sh`

2. Press enter (possibly a few times)
3. Type out `yes` to all yes or no questions
4. Hit enter and have Anacondas install in the default home/ubuntu location
5. You should now see a ton of output starting with:
`Unpacking payload ...`
6. Exit the conda >>> by typing `exit()`

# Launch Jupyter on VM
1.  In your ubuntu user terminal, type `jupyter notebook --no-browser`
You should see some output starting with `[I 23:36:17.895 NotebookApp]` like when you normally start a notebook locally.
2. Copy and paste the URL towards the end out of the output with localhost 8888 into a new browser, the text should look like:`http://localhost:8888/?token=935637ec396cd7d99df2c5e5fe594994d68aca63c98a7b00
`
3. You'll likely get a password screen or inability to connect. That's fine, leave that alone for now.

# Map local port to VM
We will now map an arbitrary port 54321 on your local to port 8888 on the VM.

1. In a new terminal window, in any directory, write
ssh -i ~/.ssh/YOUR.pem -NL 54321:localhost:8888 ubuntu@xx.xxx.xxx.xxx
Here's mine for reference:

`ssh -i ~/.ssh/Sami_AWS_Key_Metis.pem -NL 54321:localhost:8888 ubuntu@18.217.227.199`

2. you should not see any output from the terminal, but look up at your tab name and ensure it has the ubuntu@xx.xxx.xxx.xxx at the end of it now.
3. Finally, go back to your tab in the browser with the jupyter notebook you pasted in the last section, delete `8888` in the address, and type in `54321` (so that you have `http://localhost:54321/tree`) refresh the page.

Congrats you now have the power of AWS to run your nasty Python code!
