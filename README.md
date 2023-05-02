<img width="566" alt="image" src="https://user-images.githubusercontent.com/118978642/235697927-b157c5d1-edcd-4912-9f5f-21501b0c7e22.png">
### Jenkins

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
