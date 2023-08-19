
## Introduction:
In this hands on demo i will deploy a full stack applictaion consisting a frontend and a backend application within a virtual private cloud(VPC) on Google cloud platform(GCP). There will a virtual machine(VM) for the frontend application and two VM’s for the backend application. Additionally i will set up a front-facing load balancer that will serve as a reverse-proxy for the system and distribute traffic evenly between the two backend VM’s. Lastly, i will fetch data from the backend and display it on the frontend.

## What is VM?
VM stands for Virtual Machine. It is a software emulation of a physical computer that runs an operating system and applications as if it were a separate physical machine. Virtual machines are created by virtualization software that allows multiple virtual machines to run on a single physical host machine.

Popular virtualization solutions include VMware vSphere, Microsoft Hyper-V, Oracle VirtualBox, and open-source solutions like KVM (Kernel-based Virtual Machine) and Xen.

## What is VPC?
VPC stands for Virtual Private Cloud. It is a cloud computing concept and service offered by various cloud providers like Amazon Web Services (AWS), Microsoft Azure, Google Cloud Platform (GCP), and others. A VPC allows users to create a virtual network in the cloud, which simulates the functionalities of a traditional on-premises network within a cloud environment.

It’s important to note that the specifics of a VPC can vary depending on the cloud provider you use. For example, AWS’s VPC might have slightly different features and terminology compared to Azure’s VNet (Virtual Network) or GCP’s VPC. However, the underlying concept of creating a private virtual network within the cloud remains consistent across these providers.

## What is GCP?
GCP stands for Google Cloud Platform. It is a suite of cloud computing services and products offered by Google. GCP provides a wide range of cloud-based solutions that enable businesses and developers to build, deploy, and scale applications and services effectively. As with other major cloud providers like Amazon Web Services (AWS) and Microsoft Azure, GCP offers various services across multiple domains, including computing, storage, databases, machine learning, networking, and more.

## What is Nginx?
Nginx (pronounced “engine-x”) is a high-performance, open-source web server and reverse proxy server. Originally developed by Igor Sysoev in 2004, Nginx is now maintained and developed by Nginx, Inc. It is one of the most popular web servers in the world and is widely used for serving web content, handling application delivery, and acting as a reverse proxy for load balancing and caching.

Nginx is widely used by many high-traffic websites, web applications, and content delivery networks (CDNs) due to its efficiency, speed, and ability to handle heavy workloads. It is often deployed in combination with other technologies like PHP, Node.js, Ruby, and others to create powerful and scalable web architectures.

In addition to the open-source version, Nginx, Inc. offers commercial products and services, including Nginx Plus, which provides additional features and support for enterprise use cases.

## what is load balancer?
A load balancer is a crucial component in computer networking and web services architecture. It is designed to distribute incoming network traffic across multiple servers or resources to ensure efficient utilization of resources, optimize performance, and maintain high availability of applications and services. Load balancing is particularly important in scenarios where a single server or resource might become overwhelmed by incoming requests, leading to slow response times or service downtime.

Load balancers can operate at various layers of the networking stack, such as the application layer, transport layer, or network layer.

Load balancers can improve system performance, scalability, and fault tolerance by spreading the load across multiple servers. Additionally, they can detect when a server becomes unresponsive and stop sending traffic to it, thus enhancing the overall availability of services.

Modern load balancers often come with additional features such as health checks, SSL termination, session persistence, and content caching. These features contribute to the overall reliability, security, and performance optimization of web applications and services.

## Overview of the full diagram below:

![Full diagram](img/1.png)

## Prerequisite:
- You will need GCP account
- Knowledge about VM instance, Subnetting, Network interface.
## Step 1:
In this step we will create one VPC. First login into GCP. You will see dashboard like this


![Google cloud playground dashboard](img/2.png)

Now, go to vpc nerwork option from left side and click on vpc network.


![VPC network option(highlighted area)](img/3.png)

Now click on create VPC Network(see the highlighted area).


![vpc network creation](img/4.png)

Now we will create our vpc. Give a vpc name(give name as your wish).


![name of the vpc](img/5.png)

Now scroll and go to the new subnet option, give a name for subnet(subnet-ja-1), select a region(us-east-1) and add ip range(i have given 10.10.0.0/24).


![creating subnet](img/6.png)

Now, go to firewall option and select all firewall option. Remember this is just a hands on demo project that’s why we have selected all firewall option, you will not try this in production project.


![Firewall option](img/7.png)

Now scroll down and click create button to create our fisrt vpc.


![create button](img/8.png)

Now see the vpc network chart, our vpc(vpc-ja-1) is listed there.


![vpc-ja-1](img/9.png)

![vpc-ja-1](img/10.png)

## Step 2:
In this step we will create four vm under this vpc. One vm for frontend application and two vm for backend application and one vm for nginx load balancer server.

Go to left hand menu. From Compute Engine click on vm instance option.


![vm instance option](img/11.png)

Now click on create instance(see the highlighted area).


![create instance](img/12.png)

## 1st VM:
Now we will create our first vm. Give a name to vm(give name as your wish), select a region(as we are creating this vm under the created vpc and our vpc is in the us-east1 region, so we will select us-east1 region), select zone(this is availability zone, i have selected us-east1-b, you can choose your own), select machine configuration(i have choosen E2, as it is low configuration and low cost, it’s a just hand’s on demo, so we don’t need any high configuration).


![1st vm creation](img/13.png)

We have to remember that our vm-frontend will not have any public ip. So, we should select external IPv4 address as none.


![External ipv4](img/14.png)

Now, scroll down below to Firewall option and choose Allow HTTP traffic and Allow HTTPS traffic both options.


![Firewall option](img/15.png)

Now go to Advance options, click on Networking option.


![Advance option](img/16.png)

Now go to network interface option select network(we will select vpc-ja-1), select subnetwork(i have selected subnet-ja-1).


![Network interface](img/17.png)

Now, scroll below and click on create button.


![create button](img/18.png)

Our first vm is created. See the figure below.


![vm-frontend](img/19.png)

Our 1st vm got the internel ip(private ip) of 10.10.0.2


![vm-frontend](img/20.png)

## 2nd VM:
Now we will create our backend1 vm for backend application.

Now again click on create instance option.

![create instance](img/21.png)

Now we will create second vm. Give a name to vm(give name as your wish), select a region(as we are creating this vm under our vpc and our vpc is in the us-east1 region, so we will select us-east1 region), select zone(this is availability zone, i have selected us-east1-b, you can choose your own), select machine configuration(i have choosen E2, as it is low configuration and low cost, it’s a just hand’s on demo, so we don’t need any high configuration).


![vm-backend1](img/22.png)

We have to remember that our vm-backend1 will not have any public ip. So, we should select external IPv4 address as none.


![external ipv4](img/23.png)

Now, scroll down below to Firewall option and choose Allow HTTP traffic and Allow HTTPS traffic both options.


![Firewall option](img/24.png)

Now go to Advance options, click on Networking option.


![Advance option](img/25.png)

Now go to network interface option select network(we will select vpc-ja-1), select subnetwork(i have selected subnet-ja-1).


![Network interface](img/26.png)

Now, scroll below and click on create button.


![Create button](img/27.png)

Our second vm(vm-backend2) is created. See the figure below.

![vm-backend1](img/28.png)

Our 2nd vm got the internel ip(private ip) of 10.10.0.3 and does not have any public ip.


![vm-backend1](img/29.png)

## 3rd VM:
Now we will create our backend2 vm for backend application.

Now again click on create instance option.


![create instance](img/30.png)

Now we will create third vm. Give a name to vm(give name as your wish), select a region(as we are creating this vm under our vpc and our vpc is in the us-east1 region, so we will select us-east1 region), select zone(this is availability zone, i have selected us-east1-b, you can choose your own), select machine configuration(i have choosen E2, as it is low configuration and low cost, it’s a just hand’s on demo, so we don’t need any high configuration).


[vm-backend2](img/31.png)

We have to remember that our vm-backend2 will not have any public ip. So, we should select external IPv4 address as none.


![external ipv4](img/32.png)

Now, scroll down below to Firewall option and choose Allow HTTP traffic and Allow HTTPS traffic both options.


![Firewall option](img/33.png)

Now go to Advance options, click on Networking option.


![Advance option](img/34.png)

Now go to network interface option select network(we will select vpc-ja-1), select subnetwork(i have selected subnet-ja-1).


![Network interface](img/35.png)

Now, scroll below and click on create button.


![Create button](img/36.png)

Our third vm(vm-backend2) is created. See the figure below.


![vm-backend2](img/37.png)

![vm-backend2](img/38.png)

## 4th VM:
Now we will create our load balancer vm for reverse proxy.

Now again click on create instance option.


![create instance](img/39.png)

Now we will create fourth vm. Give a name to vm(give name as your wish), select a region(as we are creating this vm under our vpc and our vpc is in the us-east1 region, so we will select us-east1 region), select zone(this is availability zone, i have selected us-east1-b, you can choose your own), select machine configuration(i have choosen E2, as it is low configuration and low cost, it’s a just hand’s on demo, so we don’t need any high configuration).


![4th vm](img/40.png)

We have to remember that only our vm-loadbalancer will have public ip. So, we should select external IPv4 address as ephimeral.


![external ipv4](img/41.png)

Now, scroll down below to Firewall option and choose Allow HTTP traffic and Allow HTTPS traffic both options.


![Firewall option](img/42.png)

Now go to Advance options, click on Networking option.


![Advance option](img/43.png)

Now go to network interface option select network(we will select vpc-ja-1), select subnetwork(i have selected subnet-ja-1).


![Network interface](img/44.png)

Now, scroll below and click on create button.


![Create button](img/45.png)

Our fourth vm(vm-loadbalancer) is created. See the figure below.


![vm-loadbalancer](img/46.png)

![vm-loadbalancer](img/47.png)

## Step 3:
In this step we will install nginx in vm-loadbalancer(4th-vm) and also install a react application in our vm-frontend(1st-vm). Then we will try to connect load balancer vm with front end vm.

## Installing Nginx in vm-loadbalancer:
Open ssh terminal of vm-loadbalancer


![vm-loadbalancer ssh terminal](img/48.png)

Authorize to open terminal.


![Authorize](img/49.png)

now type this command in ssh shell

```ruby
sudo apt update -y
```

![load balancer](img/50.png)

It will update our applications. Now type this command.

```ruby
sudo apt install nginx -y
```

![installing nginx](img/51.png)

Now copy the external ip of vm-loadbalancer and paste it in the browser url section and type enter button.


![copy external ip](img/52.png)

You will see the welcome page from nginx server.


![Nginx welcome page](img/53.png)

So, we have successfully installed nginx.

## Installing React app in vm-frontend:
Open the vm-frontend ssh terminal.


![ssh terminal](img/54.png)

Click on authorize.


![Authorize](img/55.png)

Now in terminal type

```ruby
sudo apt update -y
```

It will show an error


![Error](img/56.png)

Because the vm-frontend does not have a public ip to connect outside vpc.

So, for this we need to create a NAT Gateway for this three vm(vm-frontend, vm-backend1, vm-backend2). All the three vm will install any software through this NAT Gateway. Remember that our vm-loadbalancer will not connect through NAT Gateway, because it has already bind with a public ip.

Now search cloud nat in search bar and click on it.


![Cloud NAT](img/57.png)

Click on Create cloud nat gateway to create nat gateway.


![Create cloud nat gateway](img/58.png)

Give a a name for nat gateway. Remember, to create nat gateway we need to create cloud router. Select cloud router network(vpc-ja-1), region(us-east1). Click on Create router.


![Nat gateway](img/59.png)

Click on create new router


[create new router](img/60.png)

Give a name(router-ja). Click on create button.


![Create router](img/61.png)

Router has been created.


![Cloud router](img/62.png)

Now, click on create button.


![cloud NAT](img/63.png)

See the figure. Our cloud NAT is showing in the table.


![nat gateway](img/64.png)

![NAT Gateway](img/65.png)

Now open vm-frontend ssh terminal again. Type this again

```ruby
sudo apt update -y
```

Now it will update.


![vm-frontend](img/66.png)

Type this to become super user.

```ruby
sudo su
```

![super user](img/67.png)

Now we will have to install node js. Because to run a react application node js will be required. Type this command

```ruby
curl -fsSL https://deb.nodesource.com/setup_18.x | bash - &&\
apt-get install -y nodejs
```


![Installing node js](img/68.png)

Type exit to logout from super user.


![exit from super user](img/69.png)

We will check if node js is installed or not. Type this command

```ruby
node -v
```

Node js is installed successfully.


![Node js version](img/70.png)

Now we will check if yarn package manager is installed or not. Type this command

```ruby
yarn -v
```

It’s showing yarn is not installed.


![yarn version](img/71.png)

Actually yarn is installed but it’s not showing. We just have to enable corepack. Type this command.

```ruby
sudo corepack enable
```


![enabling corepack](img/72.png)

Now again type this command.

```ruby
yarn -v
```

Now, yarn is showing version.


![yarn version](img/73.png)

Now we will create our react app using yarn package manager. Type this command.

```ruby
yarn create vite
```


![creating react-app](img/74.png)

Give a project name(i have given our-frontend) and click on enter button. Now choose react by using arrow key and type enter button.


![react app](img/75.png)

Now choose typescript+swc and click enter button.


![typescript+swc](img/76.png)

![react app](img/77.png)

One react app has been created in a folder. This folder name is our-frontend(this is the react app name). Now go to this folder. Type this

```ruby
cd our-frontend
```

type ls in terminal and press enter button.

![react app](img/78.png)

we don’t have any node modules in our-frontend folder. To run a react app node modules libraries and packages are required. To install node modelues packages type yarn in terminal and press enter button.


![installing node modules](img/79.png)

type ls in terminal, you will see node_modules folder.


![node modules folder](img/80.png)

Now type this command to build our react app.

```ruby
yarn run build
```


![building react app](img/81.png)

so our react application has been built.

## Step 4:
In this step we will connect our vm-frontend from our vm-loadbalancer.

First open vm-frontend ssh terminal and go to already installed react app folder. Type this command

```ruby
cd our-frontend
```

now we are in react app folder. Now type this command.

```ruby
sudo yarn preview --host --port 80
```

It will open port 80 to listen any server.


![vm-frontend terminal](img/82.png)

Now open ssh terminal of vm-loadbalancer and type this command.

```ruby
sudo apt install -y telnet net-tools
```


![vm-loadbalancer terminal](img/83.png)

Now, type this command

```ruby
curl 10.10.0.2
```

Remember 10.10.0.2 is the private ip of vm-frontend. This is showing the html format of react app in the terminal.


![vm-loadbalancer terminal](img/84.png)

So we have successfully connected vm-frontend from vm-loadbalancer.

## Step 5:
In this step we will try to configure vm-loadbalancer such that when we will enter the vm-loadbalancer public ip(34.23.154.227) in the browser, it will show the vm-frontend(it will show the react app).

First go to /etc/nginx folder. Type this command

```ruby
cd /etc/nginx
```

![nginx folder](img/85.png)

Now we have to open the nginx.conf file using vim editor

![nginx.conf](img/86.png)

Type this command

```ruby
sudo vim nginx.conf
```

![nginx.conf](img/87.png)

![nginx.conf](img/88.png)


Now delete all the code in editor.

![nginx.conf](img/89.png)


Copy and paste this code in the editor. Then save and quit from vim editor.

```ruby
events {
    # empty placeholder
}


http {

    server {
        listen 80;

        location / {
            proxy_pass http://frontend;
        }

    }

    upstream frontend {
        server 10.10.0.2:80;
    }
}
```

In this configuration we are trying to bypass vm-frontend via vm-loadbalancer.

![nginx.conf](img/90.png)

Now save and quit

![nginx.conf](img/91.png)


reload server to change the config file

![reload server](img/92.png)

Now enter the vm-loadbalancer public ip(34.23.154.227, please see the figure , the ip is different because i have tried several times creating this vm, so it changes, you just insert your vm public ip) in the browser


![browser](img/93.png)

So successfully we have connected our frontend with load balancer.


![Connecting frontend with loadbalancer](img/94.png)

## Step 6:
In this step we will install express server in our two backend vm.

### Installing Express in vm-backend1:
open the ssh terminal of vm-backend1 and type this command

```ruby
sudo apt update -y
```

![backend1](img/95.png)

Now we will install node js in our vm-backend1, type this command

```ruby
sudo su

curl -fsSL https://deb.nodesource.com/setup_18.x | bash - &&\
apt-get install -y nodejs
```

![nodejs](img/96.png)

now exit from super user.

![super user](img/97.png)

make a folder be1 and go to this folder

```ruby
mkdir be1
cd be1
```

![make dir](img/98.png)

enable corepack and type this command

```ruby
sudo corepack enable
npm init -y
```

![corepack](img/99.png)

now install express, type this command

```ruby
yarn add express
```
![installing express](img/100.png)


now create a index.js file using vim editor, type this command

```ruby
vim index.js
```

![index.js](img/101.png)

copy and paste this code in the index.js file

```ruby
const express = require('express');

// Create an instance of the Express application
const app = express();

// Define a route for the root URL
app.get('/', (req, res) => {
  res.send('Hello from backend 1!');
});

// Start the server and listen on a specific port
const PORT = 80;
app.listen(PORT, () => {
  console.log(`Server is running on port ${PORT}`);
});
```

now save and quit from vim editor

![index.js](img/102.png)

type this command to run index.js file

```ruby
sudo node index.js
```

![index.js](img/103.png)

Now we will check from vm-backend2 if our vm-backend1 is running or not. Open ssh terminal of vm-backend2 and type this command

```ruby
curl 10.10.0.3
```
![curl](img/104.png)

it’s showing hello from backend1.

## Installing Express in vm-backend2:
open the ssh terminal of vm-backend2 and type this command

```ruby
sudo apt update -y
```
![vm-backend2](img/105.png)

now we will install node js in our vm-backend2, type this command

```ruby
sudo su

curl -fsSL https://deb.nodesource.com/setup_18.x | bash - &&\
apt-get install -y nodejs
```

![node js](img/106.png)

make a folder be2 and go to this folder and enable corepack

```ruby
mkdir be2
cd be2
sudo corepack enable 
```

![make dir](img/107.png)

now type this command

```ruby
npm init -y
```

![node](img/108.png)

install express using yarn

```ruby
yarn add express
```

![express](img/109.png)

create a index.js file using vm editor

```ruby
vim index.js
```

![index.js](img/110.png)

copy and paste this code in index.js file

```ruby
const express = require('express');

// Create an instance of the Express application
const app = express();

// Define a route for the root URL
app.get('/', (req, res) => {
  res.send('Hello from backend 2!');
});

// Start the server and listen on a specific port
const PORT = 80;
app.listen(PORT, () => {
  console.log(`Server is running on port ${PORT}`);
});
```

save and quit from vim editor

![vim editor](img/111.png)

now run index.js file

```ruby
sudo node index.js
```

![index.js](img/112.png)

now we will check from vm-backend1 if vm-backend2 is running or not

type this command

```ruby
curl 10.10.0.4
```

here 10.10.0.4 is the private ip of vm-backend2

![curl](img/113.png)

it’s showing “Hello from backend 2"

## Step 7:
In this step we will configure load balancer in such that we can excess backend 1 and backend 2 from our load balancer.

First run the two backend server


![backend 1](img/114.png)

![backend 2](img/115.png)

Now open load balancer terminal and go to etc/nginx folder. Type this command

```ruby
cd /etc/nginx
```

now open nginx.conf file

```ruby
sudo vim nginx.conf
```

![nginx.conf](img/116.png)

copy and paste this code in node.conf file

```ruby
events {
    # empty placeholder
}


http {

    server {
        listen 80;

        location / {
            proxy_pass http://frontend;
        }
        location /api/ {
            rewrite ^/api/(.*)$ /$1 break;
            proxy_pass http://backend;
        }
    }

    upstream frontend {
        server 10.10.0.2:80;
    }
    upstream backend{
        server 10.10.0.3;
        server 10.10.0.4;
    }
}
```

We are adding private ip of backend 1 and backend 2 in nginx.conf file

![nginx.conf](img/117.png)

now save and quit from vim editor and reload the nginx server to save the change of nginx.conf file

```ruby
sudo nginx -s reload
```

![nginx.conf](img/118.png)

now go to browser and type

```ruby
34.23.45.253/api
```

![browser](img/119.png)

It’s showing backend 1. Now again reload the browser

![browser](img/120.png)

It’s showing from backend 2. So, nginx server is equally distributing traffic between the backend server.

## Step 8:
In this step we will connect the two backend express server with our front end react app through vm-loadbalancer.

Open ssh terminal of vm-frontend and go to our-frontend folder. Then open the package.json file using vm editor. Type this command

```ruby
cd our-frontend

vim package.json
```

![package.json](img/121.png)

Now add this line in package.json file

```ruby
"proxy": "34.23.45.253/api"
```

See the line number 12. Here 34.23.45.253 is the public ip of load balancer


![package.json](img/122.png)

save and quit package.json file. Now go to src folder and install axios

and add a file ApiComponent.tsx using vim editor

```ruby
cd src

yarn add axios

vim ApiComponent.tsx
```

![axios](img/123.png)

![apicomponent](img/124.png)

copy and paste this code in ApiComponent.tsx file

```ruby
import { useState, useEffect } from 'react';
import axios from 'axios';

function ApiComponent() {
  const [data, setData] = useState([]);

  useEffect(() => {
    // Make a GET request to your Express API endpoint
    axios.get('http://34.23.45.253/api/') // Change '/api/data' to your actual API endpoint
      .then(response => {
        setData(response.data);
      })
      .catch(error => {
        console.error('Error fetching data:', error);
      });
  }, []);

  return (
    <div>
      <h1>Data from Express API:</h1>
      <h1>{data}</h1>
    </div>
  );
}

export default ApiComponent;
```

In this code we are calling load balancer using axios. now save and quit from vim editor


![ApiComponent.tsx](img/125.png)

Now open the App.tsx file

```ruby
vim App.tsx
```

Copy and paste this code

```ruby
import ApiComponent from './ApiComponent';

function App() {
  return (
    <div>
      <h1>My React App</h1>
      <ApiComponent />
    </div>
  );
}

export default App;
```

Save and quit this file


![App.tsx](img/126.png)

![App.tsx](img/127.png)

Now run and build again to save the changes

```ruby
yarn run build
```

![build](img/128.png)

now go to src folder

```ruby
cd src
```

![src folder](img/129.png)

Type this command

```ruby
sudo yarn preview --host --port 80
```

![frontend](img/130.png)

![frontend](img/131.png)

Now open browser and enter the public ip of load balancer

![loadbalancer](img/132.png)

It’s showing Hello from backend 1. Reload this page again

![loadbalancer](img/133.png)

It’s showing Hello from backend 2.

Finally we have done our project successfully.


![Full diagram](img/134.png)
