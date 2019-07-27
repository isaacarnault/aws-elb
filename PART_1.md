## Part 1 : create two EC2 instances

- We log into our `AWS` management console using `$ https://console.aws.amazon.com.`<br>

I'm using `MFA` to secure my root account access coupled with `Google Authenticator` on my `Android` smartphone.<br>

We can bypass this step and login normally to `AWS` Management Console.<br>

<details>
<summary>🔴 See output</summary>
<p>  

[![isaac-arnault-AWS-1.jpg](https://i.postimg.cc/L5F2KQwp/isaac-arnault-AWS-1.jpg)](https://postimg.cc/nj26q2nR)

</p>
</details>

- We go to Services > EC2 > Launch Instance > choose t2.micro > Next: Configure Instance Details<br>

- Subnet: select "Default in us-east-1a"<br>

<details>
<summary>🔴 See output</summary>
<p>  

[![isaac-arnault-AWS-81.png](https://i.postimg.cc/QMpzX7gC/isaac-arnault-AWS-81.png)](https://postimg.cc/8Fz44j0g)

</p>
</details>

- We are going to use a custom script to launch our `EC2` instance:<br>

- Click "Advanced Details"<br>

Ues the following Script<br>

<details>
<summary>🔵 See script</summary>
<p>  
  
#!/bin/bash<br>
yum update -y<br>
yum install httpd -y<br>
service httpd start<br>
chkconfig httpd on<br>
cd /var/www/html<br>
echo "<html><h1>This is Web Server 1"</h1></html>" > index.html

</p>
</details>

<details>
<summary>🔴 See output</summary>
<p>  

[![isaac-arnault-AWS-82.png](https://i.postimg.cc/j5tFCjvX/isaac-arnault-AWS-82.png)](https://postimg.cc/FfCZBNMd)

</p>
</details>

- Next: Add Storage (leave as it is by default) > Next: Add Tags<br>

Key: Name > Value: my-WEB-Server-1 > Next: Configure Security Group > Create a new security group<br>

Security group name: SG-ELB > Description: Security Group of my ELB > Add rule: enable HTTP and HTTPS<br>

<details>
<summary>🔴 See output</summary>
<p>  
  
[![isaac-arnault-AWS-87.png](https://i.postimg.cc/Kv4fQWzx/isaac-arnault-AWS-87.png)](https://postimg.cc/mtf7g8C5)

</p>
</details>

You can create a new key pair or Choose an existing key pair. I'll choose an existing Key Pair since I've already got one.<br>

The Key Pair is saved in a folder named SSH on my Desktop. <br>

Verify that <b>my-WEB-Server-1</b> is running.<br>

<details>
<summary>🔴 See output</summary>
<p>  

[![isaac-arnault-AWS-85.png](https://i.postimg.cc/8c26Z7FQ/isaac-arnault-AWS-85.png)](https://postimg.cc/4H1ydNbB)

</p>
</details>

Let's create a second EC2 instance before we enable `ELB`.

Click on "Launch instance"<br>

Select Amazon Linux 2 AMI > t2.micro > Configure Instance Details > Subnet: choose <b>us-east-1b</b><br>

<details>
<summary>🔴 See output</summary>
<p>  

[![isaac-arnault-AWS-86.png](https://i.postimg.cc/KjPqbk6F/isaac-arnault-AWS-86.png)](https://postimg.cc/SYK77sQt)

</p>
</details>

Advanced details: use custom script below<br>

<details>
<summary>🔵 See script</summary>
<p>  
  
#!/bin/bash<br>
yum update -y<br>
yum install httpd -y<br>
service httpd start<br>
chkconfig httpd on<br>
cd /var/www/html<br>
echo "<html><h1>This is Web Server 1"</h1></html>" > index.html

</p>
</details>

- Next: Add Storage (leave as it is by default) > Next: Add Tags<br>

Key: Name > Value: my-WEB-Server-2 > Next: Configure Security Group > Use an existing Security Group<br>

We choose our existing Security Group which is <b>SG-ELB</b><br> then "Review and Launch" > Launch.

<details>
<summary>🔴 See output</summary>
<p>  

[![isaac-arnault-AWS-87.png](https://i.postimg.cc/Kv4fQWzx/isaac-arnault-AWS-87.png)](https://postimg.cc/mtf7g8C5)

</p>
</details>

We choose an existing key pair since we've already got one.<br>

Verify that <b>my-WEB-Server-2</b> is running.<br>

<details>
<summary>🔴 See output</summary>
<p> 

[![isaac-arnault-AWS-88.png](https://i.postimg.cc/jd5Wf0jQ/isaac-arnault-AWS-88.png)](https://postimg.cc/c6p4qzwr)

</p>
</details>

To check that our 2 web servers <b>my-WEB-Server-1</b> and <b>my-WEB-Server-2</b> are running:<br>
- we can connect to Server 1 using it's `IPv4 address` in our browser.<br>

- we can do the same as for Server 2.

<details>
<summary>🔴 See output</summary>
<p> 

[![isaac-arnault-AWS-88.png](https://i.postimg.cc/sDrJp2Wd/isaac-arnault-AWS-88.png)](https://postimg.cc/jCMNTtvZ)

</p>
</details>
