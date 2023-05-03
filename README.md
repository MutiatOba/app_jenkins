### testing change again to test jenkins......

### what is jenkins

Jenkins is an open source continuous integration/continuous delivery and deployment (CI/CD) automation software DevOps tool written in the Java programming language. It is used to implement CI/CD workflows, called pipelines.

### Other CICD Tools
There are a number of other CICD tools

<img width="317" alt="image" src="https://user-images.githubusercontent.com/118978642/235721037-31e83e60-4aa3-4a8c-bed8-842e009dae53.png">

Continuous Integration (CI): Developers merge/commit code to master branch multiple times a day, fully automated build and test process which gives feedback within few minutes, by doing so, you avoid the integration hell that usually happens when people wait for release day to merge their changes into the release branch.

Continuous Delivery is an extension of continuous integration to make sure that you can release new changes to your customers quickly in a sustainable way. This means that on top of having automated your testing, you also have automated your release process and you can deploy your application at any point of time by clicking on a button. In continuous Delivery the deployment is completed manually.

Continuous Deployment goes one step further than continuous delivery, with this practice, every change that passes all stages of your production pipeline is released to your customers, there is no human intervention, and only a failed test will prevent a new change to be deployed to production.

<img width="526" alt="image" src="https://user-images.githubusercontent.com/118978642/235721775-9187721d-325b-4947-9cf6-b5c0c4ed6d43.png">

### CICD

1. Code development: This stage involves the actual coding of new features, bug fixes, or improvements.

2. Code review: In this stage, code changes are reviewed by other developers, who provide feedback and suggest improvements.

3. Code testing: This stage involves running automated tests to ensure that the code changes do not break existing functionality.

4. Build: In this stage, the code changes are compiled, packaged, and built into a deployable artifact.

5. Deployment: In this stage, the built artifact is deployed to a testing environment or a production environment, depending on the pipeline configuration.

6. Testing: In this stage, the application is tested in the target environment to ensure that it is functioning correctly and meeting the desired requirements.

7. Release: In this stage, the application is released to end-users or customers, depending on the pipeline configuration.

8. Monitoring: In this stage, the application is monitored for performance, errors, and other issues, and data is collected to improve future releases.

<img width="566" alt="image" src="https://user-images.githubusercontent.com/118978642/235697927-b157c5d1-edcd-4912-9f5f-21501b0c7e22.png">

need github - continously developing code and integrating that code with what others are developing. Send to github via ssh

When it gets to github, you need to test it before going to next stage

You are continuously integrating.

webhook trigger - code is triggured each time an actions takes place. behind the webhook there is an API. the webhook is always actively looking. so min we send code, it triggers. We need ssh setup for this to happen in relation to a specific repository where we have the app code. 

if the code is merged succeful, we need to test the code. we use jenkins to test the code. jenkins has been set up on aws. need to put public key in github and private key in jenkins for specifi repository so when data travels it is secured. 

To test code, we have Jenksins servers. we want to automate the testing. we have master node running (ec2 instance) which is listening. when github receives the code, jenkin clones the code and create a job in jenkins and test is. Agent node run the test. if the test past master node gets results then we can go into production on aws. we have an ec2 on aws which is replaced by the new ec2. - this is where we get to delivery and deployment steps. 


Some basic preliminary steps:
- click on new item
- give your job a name: mutiat-checking-zone
- click on freestyle job - then ok
- in the description section, enter the following: Building a job to check time zone of this server
- Go to the build section- click add build steps - then clickexecute shell - and then type: date
- click save
- click on build now

You will then see job under build history.
- click on drop down
- then click on console output
- back to project
- back to dashboard

To automate we need a specific enviroment for our app 
- need ubuntu & nginx 

If we are not sure what server we have so we need to test it: 
- click on new items
- item name: mutiat-os-check
- freestryle job - ok
- description: under build section - click add build steps - click execute shell- then type: uname -a
- click save
- click build now

If we want the two steps to run automatically one after the other:
- go to your first job
- click on configure
- under post build actions (what happens if job successful or unsuccessful) - click build other projects - type name of project - trigger is build is table 
- click save
- click build now


#### Steps: generate new ssh key in ssh folder
2.1 copy the key to github repo where you have the app code
2.2 make a change to code and push to github
step 3: generate new ssh key called mutiat-jenkins - copy public key to the repo and private key to jenkins
step 4: set up webhook in github wth Jenkin's end point - jenkins IP (including the port)

##### Step 2.1, 2.2
- make sure your repo on GitHub include the app folder
- go to Gitbash app and do the following:
1. cd .ssh .Once in the folder, type the following command to create your authentication key.

ssh-keygen -t rsa -b 4096 -C "<email address>"

Make sure you enter in your own email address. The above command will create public/private rsa key pair. When prompted to enter the file in which to save the key, type the following:

<name>-jenkins-key
Press enter when prompted to enter in your password. If your keys have been properly created you should see the following image.

2. Associate your SSH keys with GitHub repository
in your github account, click on the repository, settings, deploy key and then add your public key
To access your public key, head back to your Git Bash App. Type the following command to access the relevant file:

cat <name>-jenkins-key.pub
Copy the long public key from the file and paste it in the box on GitHub. Once done, click "Add Key"

3. Add agent and authentication
Type the following commands in your Git Bash App:

eval 'ssh-agent -s'
ssh-add <name>-jenkins-key
ssh -T git@github.com

##### Step 3
- head over to jenkin
- click on new items
- enter name: mutiat-CI
- click Freestyle
- type description: building a CI for automated testing
- in the general tab, configure as follows:
<img width="584" alt="image" src="https://user-images.githubusercontent.com/118978642/235717949-d8e02e5e-d1fc-4914-94c0-7df1b2bc2f02.png">
copy http url for git repository: https://github.com/MutiatOba/app_jenkins.git
 <img width="595" alt="image" src="https://user-images.githubusercontent.com/118978642/235719074-0832e7f4-ff24-4b1a-8010-04bc5e9aa2ca.png">
- under office 365 connector
<img width="574" alt="image" src="https://user-images.githubusercontent.com/118978642/235718442-c0a4835a-0b27-46f8-8875-4523794c6090.png">
- under source code management - paste the url for ssh for repository, click add by the creditials box
click add, then configure as below - you need to add your private key to the box where it says add
 <img width="784" alt="image" src="https://user-images.githubusercontent.com/118978642/235719747-6a93b1eb-eee2-42cb-8b3b-280f3b1e8d3b.png">
- under build enviroment configure as follows:
<img width="593" alt="image" src="https://user-images.githubusercontent.com/118978642/235720200-55da9ef6-04d4-4265-9d5c-79884a279119.png">
- under build section
<img width="578" alt="image" src="https://user-images.githubusercontent.com/118978642/235720331-12012404-599b-4d9f-9646-4b6b85ab6d44.png">
 
 click save. Once on the dashboard click on build now. 
 
 #### automating steps
 
 Now want to automate. So once make change on github, dont have to press build now in Jenkins. Need to create a webhook. So need to configure github. final step, when make changes locally, webhook is triggured and jenkins is triggured. You want to check things in stages so can see where issues are. 

##### overall steps: 
 
Jenkins to github connection: 
- go to jenkins
- click on your job
- want to make change on readme (on git) - commit change - if you press commit changes then job actomatically triggured on jenkins. the webhook helped triggured this 

Check changes made locally built automatically on github:
- git pull - pull changes from git
- then make a change to readme on local host
- git add .
- git commit
- git push 
- check job triggured on jenkins 

This is a complete CI now 

 ##### detailed steps:
 
in summary we want to:
- create webhook for jenkins/endpoint. 
- create the webhook in github for the repo where you have the app code
- test the webhook - testing status code 200
- make a change to github read me and commit the change (if webhook workinng then should see job triggured)

github to jenkins connection:
- go to github repo
- setting
- webhook
- go to jenkins and copy webip: http://35.178.11.196:8080/github-webhook/
- back to git hub and configure as below:
1. application/json
2. just push event
3. add webhook
[webhook is api call in the background]
 <img width="609" alt="image" src="https://user-images.githubusercontent.com/118978642/235888265-3b19d19b-84e6-4847-84d2-9a97e4e9da30.png">


go to your job on jenkins:
- click on configure
- under build triggure
- tick: GitHub hook trigger for GITScm polling
- save
[now jenkins has been told to troggire job]
<img width="573" alt="image" src="https://user-images.githubusercontent.com/118978642/235888401-b1f5ffe5-3f91-4810-9530-7ff47fe52be1.png">

- make a change to readme on github
- commit change
- Then a job on jenkins automatically triggured

##### local to jenkins
Now go to local host 
- git pull origin main
- make a change to your readme
- git add readme.md
- git commit -m "change for jenkins"
- git push 
- change is pushed and built on jenkins 

We have already automated testing with jenkins [look at build section]
<img width="553" alt="image" src="https://user-images.githubusercontent.com/118978642/235888842-d913b53c-3d79-4b15-9c9e-68d67f6307ca.png">

if there is issue then send console output to relevant person. can save the logs with s3 
 
 
