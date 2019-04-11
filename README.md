# GitMultipleAccounts
How to use multiple git accounts on the same computer

# Generate an SSH key for each account
First check with command `ls -la ~/.ssh` to see what the existing keys are if any.

Next lets make a ssh key for a personal and work git.

`ssh-keygen -t rsa -C "workemail" -f "id_rsa_<work_user1>" ` 

`ssh-keygen -t rsa -C "personalemail" -f "id_rsa_<personal_user1>"`

Check to make sure they were created in `~/.ssh` with command `ls -la ~/.ssh`. If they were created there just move all the related files from the cwd to `~/.ssh` (there should be two files per key).

# Adding the key to Git account
Go to the corresponding git website and sign in. Then look for where you can add an ssh key (most likely in settings). Copy the .pub for that corresponding key by using `cat ~/.ssh/id_rsa_<work_user1>.pub` and copying the output into the web browser. (note: for personal the command will be `cat ~/.ssh/id_rsa_<personal_user1>.pub`).

# Register new ssh keys with ssh-agent
Make sure your ssh-agent is running with command `eval "$(ssh-agent -s)"`, then add keys to the ssh-agent with commands
```
ssh-add ~/.ssh/.ssh/id_rsa_<work_user1>
ssh-add ~/.ssh/.ssh/id_rsa_<personal_user1>
```

# SSH Config File
After adding both the keys open `~/.ssh/config` with your favorite text editor or create it if it doesn't exist. Configure the entries as shown below. Replace `<work_user1>` and `<personal_user>` with the git id for the respective account.
```
# Work account,
Host github.com-<work_user1>
   HostName github.com
   User git
   IdentityFile ~/.ssh/id_rsa_<work_user1>
# Personal account
Host github.com-<personal_user1>    
   HostName github.com
   User git
   IdentityFile ~/.ssh/id_rsa_<personal_user1>
```

# Activate an SSH key in the ssh-agent
This method works by toggling the ssh key that is in use via the ssh-agent. `ssh-add -l` will list all the SSH keys attached to the ssh-agent. You need to remove all the keys and attach the one you plan to use. `ssh-add -D` will remove all ssh entries from the ssh-agent. Then you just need to add the key you plan to use with command: `ssh-add ~/.ssh/id_rsa_<work_user1>`. This will allow you to work on your work git.

When you need to switch just do 
```
ssh-add -D
ssh-add ~/.ssh/id_rsa_<personal_user1>
```
and your good to go.

# Make sure git config is in order
You can do `git config --list` to see the git config. Make sure `user.name` and `user.email` are correct.
If they aren't you can set them with commands:
```
git config user.name "user 1"
git config user.email "user@eamil"
```
# Cloning
When you got to clone a repo make sure to use the ssh url.
