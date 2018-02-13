# DevOps Homework1_Part2

# Server Provisioning

<h4> Server provisioning and ansible scripts for creation of AWS Amazon EC2 and DigitalOcean Droplet </h4>

<h5>

I will be creating 2 instances:
1. Amazon EC2
2. Digital Ocean Droplet

<h4> Instructions on getting started </h4>

1. To get started, you need to sign up with [DigitalOcean](https://www.digitalocean.com/) ans [Amazon AWS](https://aws.amazon.com/)

2. Once you sign up with DigitalOcean, generate a token for your account and save it. The instructions for this can be found [here](https://www.digitalocean.com/community/tutorials/how-to-use-the-digitalocean-api-v2)

3. You will need the Access and Secret token for AWS. [Instructions](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html)

4. Save these tokens as Environment Variables.

```
DOTOKEN
AWS_ACCESS_KEY
AWS_SECRET_KEY
```

5. Generate ssh keys for DigitalOcean. [Instructions](https://www.digitalocean.com/community/tutorials/how-to-use-ssh-keys-with-digitalocean-droplets)

<h4> Steps for implementation </h4>

Run the following command:

```
ansible-playbook -i inventory main.yml -vvv
```

I have used 3 roles:
1. InstallDependencies - Installs all the dependencies and clones the git repo which consists entire code.
2. InstallModules - Installs required modules using npm module of ansible.
3. CreateInstances - Runs both .js files(CreateInstanes.js and CreateDroplet.js) for launching the servers and displaying the data of servers.

<h4>Demo video explaining the procedure</h4>

[Server_Provisioning_Screencast](https://youtu.be/u0Ez_FGw7Yw)

</h5>





<h2> Concepts </h2>

<h5>

<h3> 1. Define idempotency. Give two examples of an idempotent operation and non-idempotent operation. </h3>

 
Idempotence, in programming and mathematics, is a property of some operations such that no matter how many times you execute them, you achieve the same result. In programming, idempotence can be a property of many different code elements, including functions, methods, requests and statements. 
Idempotence in DevOps or any field is simply a word that explains an operation that will have the same effect whether you run it once or 1000 times.
From a RESTful service standpoint, an operation is idempotent if clients can make that same call repeatedly while producing the same result. In other words, making multiple identical requests has the same effect as making a single request.  

Idempotent examples:
1. In REST API, GET method. And also GIT PULL command.
2. Another example would be of puppet. Every time agent runs, it has same set of instructions logged in a catalog. These instruction or code tells Puppet about the state it should manage/keep e.g. state present denotes that puppet should keep that particular application/tool present in the environment.

```Clojure
package { [ putty', 'notepadplusplus']:
    ensure   => present,
    provider => 'chocolatey',
}
```

3. Ansible module
   
   eg: apt
    
   Code:
   
   
   ```YAML
   - name: Install the package "SomePackage"
     apt:
       name: SomePackage
       state: present
   ```
  
   
   If package is not present it gets installed ans if its already it gives an OK message.
   Basically state=present only checks if a package is present, if not then installs it. Hence idempotent
   
Non-Idempotent Examples:
1. PUT method and GIT PUSH command.

2. Bash Command


```Bash
echo "127.0.0.1 localhost" >> /etc/hosts
```
The above code will append ‘/etc/hosts’ entry over and over again which can be a problem


<h3> 2. Describe several issues related to management of your inventory. </h3>


Problems:

1. As complexity increase we might have to deal with numerous ip addresses and there is good chance that either we lose one of them or forget one of them and this could cause very serious failure as many a times this ip is used in hosting and testing the servers.

2. Dependency packages take a version name, if specified then there is a possibility that, that version is not available anymore. This will create problem.

3. If you don't specify a version then, there is a possibility that it will download latest one and then break the subsequent build process.

4. We sometimes have to embed passwords to access particular thing using inventory here we are exposing our password and it can cause security breach.
5. Need to maintain inventory list


<h3>3. Describe two configuration models. What are disadvantages and advantages of each model?</h3>

There are two configuration models: PUSH and PULL
<h4>
Push Model:
In PUSH model, A central server contacts the nodes and sends updates as they are needed. When a change is made to the infrastructure (code) each node is alerted of this and they run the changes.
</h4>

Advantages:

1. It is easy to manage the VMs from 1 server and can make changes with minimal time
2. Everything is synchronous, and under your control so if something goes wrong, one can correct it immediately.
3. Development is sensible

Disadvantages:

Lack of complete automation as it is not easily possible to boot a server and have it configure itself without some sort of client/server protocol that push systems don't generally support.

<h4>
Pull Model:
The pull configuration model, requires VMs to send their configuration changes to the server. 
</h4>

Advantages:

1. Flexibility as the asset can register itself to the setup and de-register easily.
2. it is possible to fully automate the configuration of a newly booted server using a 'pull' deployment system


Disadvantages:

Lot of manual work and time consuming. Also scalability is an issue.

>

<h3>4. What are some of the consquences of not having proper configuration management?</h3>


1. It is very difficult to predict and figure out the components that are constantly chnaging. 

2. If replacement of component is to be performed and it deletes or modifies existing functionalities then derived functionalities are badly affected and have serious impact on them.

3. Tokens, keys etc data must be managed. If it is not properly handled then this breach in security can cause fatal defects to the software or data we are processing.

4. Recovery issues in case of accidents.

5. Bad configuration management system can expose this private data and can cause threat to the system.

<h3> References </h3>

1. [ServersWorkshop](https://github.ncsu.edu/CSC-DevOps-Spring2015/ServersWorkshop)
2. [AWS Documentation](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/ec2-example-creating-an-instance.html)
3. [DigitalOcean Documentation](https://developers.digitalocean.com/documentation/v2/)
4. [StackOveflow](https://stackoverflow.com/questions/37137513/ansible-install-node-js-version-6)
5. Wikipedia

</h5>
