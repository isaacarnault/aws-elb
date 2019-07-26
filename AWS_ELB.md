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
  
#!/bin/bash
yum update -y
yum install httpd -y
service httpd start
chkconfig httpd on
cd /var/www/html
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

[![isaac-arnault-AWS-82.png](https://i.postimg.cc/j5tFCjvX/isaac-arnault-AWS-82.png)](https://postimg.cc/FfCZBNMd)

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
  
#!/bin/bash
yum update -y
yum install httpd -y
service httpd start
chkconfig httpd on
cd /var/www/html
echo "<html><h1>This is Web server 2"</h1></html>" > index.html

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

## Part 2 : create an Elastic Load Balancing (ELB)

### 2.1 classic way

On your `EC2` dashboard, on <b>Load balancing</b> section click on <b>Load balancers</b> > Create Load Balancer<br>

Create <b>Classic Load Balancer</b> > Load Balancer name: <b>myELB</b><br>

<details>
<summary>🔴 See output</summary>
<p> 

[![isaac-arnault-AWS-89.png](https://i.postimg.cc/52vYp5Ws/isaac-arnault-AWS-89.png)](https://postimg.cc/fVzRR9Ld)

</p>
</details>

- Next: Assign Security Groups > select an existing Security Group: <b>SG-ELB</b><br>

- Next: Configure Health Check. Here we are using a custom configuration, but you can leave the set up as it is by default.<br>

. Response timeout: 2s<br>
. Interval: 3s<br>
. Unhealthy threshold: 2s<br>
. Healthy threshold: 3s<br>

<details>
<summary>🔴 See output</summary>
<p> 

[![isaac-arnault-AWS-90.png](https://i.postimg.cc/Kv74MGPW/isaac-arnault-AWS-90.png)](https://postimg.cc/zVBJm1Pw)

</p>
</details>

- Next: Add EC2 Instances: let's select our two `EC2` instances that we've created earlier.<br>

<details>
<summary>🔴 See output</summary>
<p>

[![isaac-arnault-AWS-91.png](https://i.postimg.cc/D0TnvGMw/isaac-arnault-AWS-91.png)](https://postimg.cc/LYNrvJSc)

</p>
</details>

- Next: Add Tags > Key: Name > Value: myELB > Review and Create > Create <br>

To check that our `ELB` is working, we should click on Instances tab and wait until our two `EC2` instances status are "InService".<br>

<details>
<summary>🔴 See output</summary>
<p>

[![isaac-arnault-AWS-92.png](https://i.postimg.cc/PqXDxL0K/isaac-arnault-AWS-92.png)](https://postimg.cc/QFzCYM8W)

</p>
</details>

We can also try our load balancing on our web browser, by copy/paste `DNS Name` into our web browser.<br>

<details>
<summary>🔴 See output</summary>
<p>

[![isaac-arnault-AWS-93.png](https://i.postimg.cc/cCngjf81/isaac-arnault-AWS-93.png)](https://postimg.cc/9wcQwRps)

</p>
</details>


When refreshing our browser, Web Server 1 & 2 should alternate upon page refresh, which means that our `ELB` is running.

<details>
<summary>🔴 See output</summary>
<p>

[![isaac-arnault-AWS-94.png](https://i.postimg.cc/C1fQKLZP/isaac-arnault-AWS-94.png)](https://postimg.cc/Hcd2Zm9X)

</p>
</details>

### 2.1 advanced way

Go to EC2 > Load Balancing > Target Groups > Create target group > Target group name: myTG-1 > Path: /index.html<br>

Advanced health check settings: <br>

Healthy threshold: 2 > Unhealthy threshold: 3 > Timeout: 5 > Interval: 6 > Success codes: 200 > Create<br>

<details>
<summary>🔴 See output</summary>
<p>

[![isaac-arnault-AWS-96.png](https://i.postimg.cc/YS0xM1bN/isaac-arnault-AWS-96.png)](https://postimg.cc/06gSCJgr)

</p>
</details>

We should add our EC2 instances to our Target Group.<br>

Go to Target Groups > Select myTG-1 > Click on Targets tab > Edit > Select both instances and click Save<br>

<details>
<summary>🔴 See output</summary>
<p>

[![isaac-arnault-AWS-97.png](https://i.postimg.cc/WpqTtChC/isaac-arnault-AWS-97.png)](https://postimg.cc/V5w3V79W)

</p>
</details>

Now let's create a Load Balancer in a more robust way. Go to Load Balancers > Create Load Balancer<br>

Create a Application Load Balancer

<details>
<summary>🔴 See output</summary>
<p>

[![isaac-arnault-AWS-98.png](https://i.postimg.cc/ZqJ5dc13/isaac-arnault-AWS-98.png)](https://postimg.cc/9DN23GXF)

</p>
</details>

Name: myALB-EC2 > Availability Zones: You can select the one that fits your needs. For this gist I selected:<br>
- us-east-1a<br>
- us-east-1b<br>
- us-east-1c<br>

<details>
<summary>🔴 See output</summary>
<p>

[![isaac-arnault-AWS-97.png](https://i.postimg.cc/0NBRf1TJ/isaac-arnault-AWS-97.png)](https://postimg.cc/94ZnmKFW)

</p>
</details>

Next: Configure Security Settings > Next: Configure Security Groups > Select the SG we've created earlier <br>SG-ELB</b><br>

<details>
<summary>🔴 See output</summary>
<p>

[![isaac-arnault-AWS-99.png](https://i.postimg.cc/qqHFbJjg/isaac-arnault-AWS-99.png)](https://postimg.cc/8JwwsS8V)

</p>
</details>

- Next: Configure routing > Select existing target group

<details>
<summary>🔴 See output</summary>
<p>

[![isaac-arnault-AWS-99.png](https://i.postimg.cc/qqHFbJjg/isaac-arnault-AWS-99.png)](https://postimg.cc/8JwwsS8V)

</p>
</details>

- Next: Register Targets > Next: Review > Create

Our `Application Load Balancer` <b>myALB-EC2</b> should now appear in Load Balancers.<br>

Go to Target Groups > click on Targets tab > Edit > select all EC2 instances and click "Add to registered"<br>

<details>
<summary>🔴 See output</summary>
<p>

[![isaac-arnault-AWS-101.png](https://i.postimg.cc/4NFXP0rt/isaac-arnault-AWS-101.png)](https://postimg.cc/2Vhp80j8)

</p>
</details>

Our Application Load Balancer is now active. Go to Description and copy DNS name into your clipboard.<br>

Paste it into your browser and do some refreshing of the page. You should see it alternating between web server 1 and 2.

This mean our <b>myALB-EC2</b> is running fine.<br>

<details>
<summary>🔴 See output</summary>
<p>

[![isaac-arnault-AWS-102.png](https://i.postimg.cc/rpDjWD5x/isaac-arnault-AWS-102.png)](https://postimg.cc/jWryYdgS)

</p>
</details>
