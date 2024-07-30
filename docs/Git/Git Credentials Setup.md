###  Setting your Git username for every repository on your computer

- Lets check the config list 

```
git config --list
```

- Set a Global Git username:

```
git config --global user.name "<GitHub Username>"
```

Output: 

```
root@OpsFusionLabs-Git-Server:~/opsfusionlabs# git config --global user.name "opsfusionlabs"
root@OpsFusionLabs-Git-Server:~/opsfusionlabs# git config user.name
opsfusionlabs
root@OpsFusionLabs-Git-Server:~/opsfusionlabs#

```
- Set a Global Git user email:

```
git config --global user.email "<Git Hub Email ID>"
```

Output: 

```
root@OpsFusionLabs-Git-Server:~/opsfusionlabs# git config --global user.email "opsfusionlabs@gmail.com"
root@OpsFusionLabs-Git-Server:~/opsfusionlabs# git config user.email
opsfusionlabs@gmail.com

```
- Confirm that we have set the Git username and email correctly:

```
git config user.name && git config user.email
```

Output: 

```
root@OpsFusionLabs-Git-Server:~/opsfusionlabs# git config user.name && git config user.email
opsfusionlabs
opsfusionlabs@gmail.com

```
###### Example:  

```
git config --global user.name "opsfusionlabs"
git config --global user.email "opsfusionlabs@gmail.com"
git config --list 
```
