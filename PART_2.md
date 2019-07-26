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
