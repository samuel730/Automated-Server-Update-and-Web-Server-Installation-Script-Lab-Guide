# Automated Server Update and Web Server Installation Script

## Project Overview

This project demonstrates how to automate server maintenance and web server installation using Bash shell scripting and Vagrant virtual machines. The script remotely accesses multiple Ubuntu servers, performs system updates, and installs both Nginx and Apache2 web servers automatically.

The project highlights important DevOps concepts such as automation, remote server management, consistency in server configuration, and error handling.

---

# Objectives

The objectives of this project are:

* Automate system updates across multiple servers
* Install Nginx and Apache2 automatically
* Reduce manual configuration tasks
* Improve consistency across servers
* Demonstrate Bash scripting and SSH automation
* Practice DevOps and Linux administration skills

---

# Technologies Used

* Bash Shell Scripting
* Ubuntu Linux
* Vagrant
* VirtualBox
* SSH
* Apache2
* Nginx

---

# Prerequisites

Before starting, ensure the following are installed:

* VirtualBox
* Vagrant
* Git Bash (Windows)
* Internet connection

---

# Project Setup

## Step 1 — Create Project Folder

Open PowerShell and create a project directory:

```bash
mkdir devops-project
cd devops-project
```

---

## Step 2 — Initialize Vagrant

Run:

```bash
vagrant init
```

---

## Step 3 — Configure the Vagrantfile

Replace the contents of the `Vagrantfile` with:

```ruby
Vagrant.configure("2") do |config|

  (1..3).each do |i|
    config.vm.define "server#{i}" do |node|
      node.vm.box = "ubuntu/bionic64"
      node.vm.hostname = "server#{i}"

      node.vm.network "private_network", ip: "192.168.56.#{10+i}"

      node.vm.provider "virtualbox" do |vb|
        vb.memory = 512
        vb.cpus = 1
      end
    end
  end

end
```

<img width="1182" height="798" alt="image" src="https://github.com/user-attachments/assets/3e672f56-4232-4af1-a817-fa64cc08edf5" />

---

## Step 4 — Start the Virtual Machines

Run:

```bash
vagrant up
```

<img width="1412" height="1392" alt="image" src="https://github.com/user-attachments/assets/3c54a418-023d-434e-9b17-1fdc4a3ca3f2" />

<img width="1450" height="1400" alt="image" src="https://github.com/user-attachments/assets/d5190b8c-6c5a-4486-ac34-8a9124081e9c" />

<img width="2560" height="542" alt="image" src="https://github.com/user-attachments/assets/d540e12b-4153-435a-af20-cf10e88230e1" />

Verify the servers are running:

```bash
vagrant status
```

<img width="1250" height="322" alt="image" src="https://github.com/user-attachments/assets/8ad587b2-7b7f-48de-9eee-b1c9dcd56d4b" />

Expected output:

```bash
server1 running
server2 running
server3 running
```

<img width="1250" height="322" alt="image" src="https://github.com/user-attachments/assets/94f58f60-1b7b-4576-8253-9758686a2c9e" />

---

# Creating the Deployment Script

## Step 5 — Create deploy.sh

Create the deployment script:

```bash
notepad deploy.sh
```

<img width="1202" height="42" alt="image" src="https://github.com/user-attachments/assets/721543ec-f99f-4608-84ed-eebb74e0cf52" />

Paste the following script:

```bash
#!/bin/bash

# List of servers
SERVERS=("server1" "server2" "server3")

echo "Starting deployment..."

# Function to update and install web servers
deploy_server() {

    SERVER=$1

    echo "================================="
    echo "Connecting to $SERVER"
    echo "================================="

    vagrant ssh $SERVER -c "
        sudo apt update -y &&
        sudo apt upgrade -y &&
        sudo apt install nginx -y &&
        sudo apt install apache2 -y &&
        sudo systemctl enable nginx &&
        sudo systemctl enable apache2
    "

    # Check deployment status
    if [ $? -eq 0 ]; then
        echo "$SERVER deployment SUCCESSFUL"
    else
        echo "$SERVER deployment FAILED"
    fi
}

# Loop through all servers
for server in "${SERVERS[@]}"
do
    deploy_server $server
done

echo "Deployment completed."
```

Save and close the file.

<img width="1290" height="1224" alt="image" src="https://github.com/user-attachments/assets/e5f722d3-9aea-467b-8962-713b2216ed3b" />

<img width="972" height="552" alt="image" src="https://github.com/user-attachments/assets/f0f24345-0fcf-421c-a0a5-bf241fba3abb" />

---

# Running the Script

## Step 6 — Open Git Bash

Navigate to the project folder:

```bash
cd "/c/Users/USERNAME/Documents/devops-project"
```

<img width="990" height="86" alt="image" src="https://github.com/user-attachments/assets/5c3e0eae-ab3b-4225-a400-cabdbe1ed1f0" />


Make the script executable:

```bash
chmod +x deploy.sh
```

<img width="1118" height="76" alt="image" src="https://github.com/user-attachments/assets/2198f258-59e9-4d76-becb-9235289ec065" />


Run the script:

```bash
./deploy.sh
```

<img width="1074" height="170" alt="image" src="https://github.com/user-attachments/assets/45849aeb-dae3-4f52-b335-0a0b97270171" />

---

# Expected Output

The script should:

* Connect to each server
* Update Ubuntu packages
* Install Nginx
* Install Apache2
* Display deployment success messages

Example:

```bash
server1 deployment SUCCESSFUL
server2 deployment SUCCESSFUL
server3 deployment SUCCESSFUL

Deployment completed.
```

<img width="2560" height="1440" alt="image" src="https://github.com/user-attachments/assets/615a27c1-6873-442b-8231-b5e7170b0b26" />

<img width="2560" height="1440" alt="image" src="https://github.com/user-attachments/assets/a6fd1a66-d1bd-4b22-998b-6a5ecdbbdfaa" />

<img width="2560" height="1440" alt="image" src="https://github.com/user-attachments/assets/c317bd1c-ff85-4d35-b72d-1127335fc084" />

<img width="1794" height="1016" alt="image" src="https://github.com/user-attachments/assets/0866cadc-a5f9-40ad-aaea-610831044180" />

---

# Verification

Verify Nginx installation:

```bash
vagrant ssh server1 -c "nginx -v"
```

<img width="1048" height="118" alt="image" src="https://github.com/user-attachments/assets/630088af-80b6-421b-9f61-8449ee54a7a1" />


Verify Apache2 installation:

```bash
vagrant ssh server1 -c "apache2 -v"
```

<img width="1078" height="132" alt="image" src="https://github.com/user-attachments/assets/d3980de2-771d-4979-bcad-2c31c5d40222" />

---

# Error Handling Implemented

The script includes:

* Deployment status checks
* Success/failure messages
* Automated execution using loops
* Consistent configuration across all servers

---

# Challenges Encountered

Some common issues encountered during the project included:

* Windows and Linux line-ending conflicts
* Running Bash scripts in PowerShell instead of Git Bash
* Attempting to run Vagrant commands inside virtual machines
* Slow package installation during Ubuntu updates

These issues were resolved through troubleshooting and proper environment setup.

---

# Conclusion

This project successfully automated server updates and web server installations across multiple Ubuntu virtual machines using Bash shell scripting and Vagrant. It demonstrated practical DevOps concepts such as automation, infrastructure management, remote execution, and server configuration consistency.

The project also improved understanding of Linux administration, scripting, SSH automation, and virtualized environments.
