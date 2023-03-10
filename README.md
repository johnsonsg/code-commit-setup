# Setting Up AWS Code Commit
Setting up AWS CodeCommit for SSH

```
cd $HOME/.ssh
ssh-keygen
[ here just create the name codecommit_rsa and leave all fields blank *just click enter* ]
[ Here, you don't need to enter a passphrase at this time, so *just click enter twice* ]
ls codecommit_rsa*
[ Then you should see two files: codecommit rsa and codecommit_rsa.pub ]
cat codecommit_rsa.pub
copy SSH key
```
```
Go to AWS IAM management console and past in copied Key
```

Now we need to enter our codecommit_rsa.pub into IAM.

```
touch config
chmod 600 config
```

```
nano config

Host git-codecommit.*.amazonaws.com
  User [YOUR_SSH_KEY_ID_FROM_IAM]
  IdentityFile ~/.ssh/codecommit_rsa
```
  

Now lets test to verify it works.

```
ssh git-codecommit.us-east-1.amazonaws.com
or you can add your ssh key like below. Either works.
ssh Your-SSH-Key-ID@git-codecommit.us-east-2.amazonaws.com
```

## You should see.
You have successfully authenticated over SSH. You can use Git to interact with AWS CodeCommit. Interactive shells are not supported.Connection to git-codecommit.us-east-1.amazonaws.com closed by remote host.

## Now you can jump back to your Directory and clone repo
```
cd $HOME
copy SSH url
git clone 'SSH_URL_COPIED'
[ You should see a message saying: warning: You appear to have cloned an empty repository. ]
[ Now you can create a file.txt and push to Master ]
git push origin master
```

## ReTest Success (optional) on command line
```
cd ../
rm -rf 'REPOSITORY_NAME' ie. name you set up for directory
[ example: rm -rf codecommit/ ]
git clone 'SSH_URL_COPIED'
[ Now, you should see repo cloned with txt file previously created. ]
```


# That's It!!!

## Troubleshooting
If you are asked for a fingerprint, click 'Yes'

### Fix aws codecommit unable to access repository (The requested URL returned error: 403

when cloning a git this error was produced
fatal: unable to access 'https://git-codecommit.us-east-1.amazonaws.com/v1/repos/RepositoryName': The requested URL returned error: 403

to fix add the following git configuration
git config --global credential.helper "!aws codecommit credential-helper $@"
git config --global credential.UseHttpPath true

check if git has system credential.helper by typing 
git config --system -l

if it has remove it by typing
git config --system --unset credential.helper


### Watch Tutorial
https://www.youtube.com/watch?v=CxKbAxV0Hno


