# Jenkins Installation Guide

This guide provides step-by-step instructions to install **Jenkins** on an Ubuntu EC2 instance and configure it with Docker.

---

## 📌 Pre-Requisites
- An **AWS EC2 instance** running Ubuntu.
- **Java (JDK 17)** installed.

---

## 🔹 Step 1: Install Java
```bash
sudo apt update
sudo apt install openjdk-17-jre
```

✅ Verify Java installation:
```bash
java -version
```

---

## 🔹 Step 2: Install Jenkins
Add the Jenkins repository key and source list:
```bash
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee   /usr/share/keyrings/jenkins-keyring.asc > /dev/null

echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]   https://pkg.jenkins.io/debian binary/ | sudo tee   /etc/apt/sources.list.d/jenkins.list > /dev/null
```

Update packages and install Jenkins:
```bash
sudo apt-get update
sudo apt-get install jenkins
```

---

## 🔹 Step 3: Configure AWS Security Group
By default, Jenkins runs on **port 8080**.  
Open **port 8080** in your EC2 instance security group:

1. Go to **EC2 > Instances > Select your Instance**  
2. Scroll to **Security Groups > Inbound Rules**  
3. Add rule: **Custom TCP Rule → Port 8080 → 0.0.0.0/0 (or your IP for security)**  

---

## 🔹 Step 4: Access Jenkins
Visit in browser:
```
http://<ec2-instance-public-ip>:8080
```

Get the Jenkins admin password:
```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

- Paste the password to unlock Jenkins  
- Click **Install Suggested Plugins**  
- Create your **Admin User** (recommended)  

✅ Jenkins installation is now complete!

---

## 🔹 Step 5: Install Docker Pipeline Plugin
1. Go to **Manage Jenkins > Manage Plugins**  
2. In **Available Plugins**, search for **Docker Pipeline**  
3. Install and restart Jenkins  

---

## 🔹 Step 6: Install Docker on EC2
Run the following commands:
```bash
sudo apt update
sudo apt install docker.io
```

Grant permissions to Jenkins and Ubuntu users:
```bash
sudo su -
usermod -aG docker jenkins
usermod -aG docker ubuntu
systemctl restart docker
```

Switch to Jenkins user and verify:
```bash
su - jenkins
```

Restart Jenkins:
```
http://<ec2-instance-public-ip>:8080/restart
```

---

## 🎉 Jenkins Installation is Successful!
You can now start creating Jenkins pipelines and integrate with Docker.
