# Knowledgebase Document
This repository contains an ongoing list of knowledge base items that I've learned over the years

## Table of Contents
- [Git Commands](#git-commands)
- [SQL Queries](#sql-queries)
- [Shell Scripting](#shell-scripting)
- [MongoDB](#mongodb)
- [Java](#java)
  - [Intellij IDEA Shortcuts](#intellij-idea-shortcuts)
  - [Maven](#maven)
  - [Spring](#spring)
    - [REST](#rest)
  - [JPA/Hibernate](#jpa-hibernate)
- [Other](#other)

## Git Commands
If errors related to invalid proxy when trying to run git pull:
From git bash, run “cd ~” to go to home dir and look for “.gitconfig” file.  In there, you may have an [http] section with a proxy defined.  To fix the problem, you have to edit or remove that.

How to list all the configuration for Git:
`git config --list`

How to set VI as the editor for Git so that when pulling down new code I can have a chance to edit the commit message without opening VS Code:
`git config --global core.editor vim`

How to make a branch from a tag:
	`
	git checkout -b <new branch name> <tag name>
	git checkout -b hotfix-68 q2oNA-Q2O_NA_DEV_Deploy-68
	`
To diff to branches excluding certain folders - in this case, I want to diff the master and current branch:
	`git difftool master 4.6PricingWSDL  -- . ':!java2ts' ':!jaxbWsdlProject'`
	
to diff with the remote branch and show file names only:
	`git diff --name-only master origin/master -- . ':!java2ts' ':!jaxbWsdlProject'`
	
Here is a command to attach a local branch to an up-stream branch:
	`git push --set-upstream origin remove-ibm-software`
	
To test a merge with no commit:
	`git merge --no-commit --no-ff remove-ibm-software`
	
To remove a remote branch:
	`git push origin --delete <remote branch name>
	for example:
	git push origin --delete remove-ibm-software`
	
To see how far out of sync with origin/master:
	`git fetch origin
	git diff --name-only origin/master`
	
To diff a particular file with origin/master:
	`git fetch origin
	git diff origin/master <directory path>/<file name>
	e.g.
	git diff origin/master q2oBusiness/src/com/blah/q2o/service/quoteservice/dao/model/DBLnvQuoteOrderHeader.java`
	
Get a list of all deleted files:
	`git log --diff-filter=D --summary | grep delete`
	
Then in order to find out all the commits related to a specific file:
	`git log --oneline --follow -- q2oBusiness/src/com/blah/q2o/service/parserservice/ibm/LenovoServicesXlsParser.java`

To force a revert of changes (in the middle of a merge) back to HEAD
	`git reset --hard HEAD`
	
To put your local branch back to a particular commit:
	`git reset --hard 029842e`
	
To add and commit at same time 
```
	commit -am “My commit comment”
```
Check the status of your local working directory (see uncommitted files, etc)
	`git status`

Branches:
What branch is active on your working directory: `git branch`

Change working directory to another branch: 
`git checkout <branch name>`
	
To create a new local branch: 
`git checkout -b <branch name>`
	
If git branch is only showing master and not other branches, use `git branch -a`

To checkout the remote branch you want, run something like this:

`git checkout origin/ASAPQuote_Jboss --track`

To associate new local branch with remote location: 

`git push --set-upstream origin <local branch name>`

To get code from remote origin via merge method

`git fetch then git merge`

1.	To abort a merge:
	`git merge –abort`

2.	Here is how you can tell difference between your local branch and what is in gitlab:

	`git diff ESAPQuote_Jboss remotes/origin/ESAPQuote_Jboss`

3.	To add uncommitted files to your local staging area, use: `git add .`

4.	How to discard untracked directories and files in the current working directory:
	`git clean -fd`

5.	How to untrack a file that is already tracked, but you don’t want to lose the changes: 
	`git rm --cached q2oEUR/.settings/org.eclipse.wst.common.component`

6.	To push your code to remote
	`git push or git push origin master or git push origin <other branch>`

7.	To produce a list of history of all branches you have locally
	`git log --oneline --graph --decorate --all`

8.	Get back to your last commit, 1st run command in #7 & get ID of your last commit, then:
	`git reset --hard 029842e (this is the ID of a particular commit)`

9.	To reset your local code to what is in local head:
	`git reset --hard HEAD`

10.	To set your local master exactly equal to the remote master (discarding any local changes), use this command:
	`git reset --hard origin/master`

11.	Note – if you fetch/merge (or pull) and you’re getting permission denied messages and files are being deleted which you know shouldn’t be deleted, close down your IDE’s (Eclipse/VS Code), revert back to your last commit and try again. The IDE’s or server may have file locks.

12.	To check your git configuration:
	`git config --global --list`

13.	To update your git configuration:
	`vi ~/.gitconfig`

14.	To see how far out of sync with origin/master
	`git fetch origin; git diff origin/master <path>/<file name>`

15.	To test a merge with no commit
	`git merge --no-commit --no-ff <branch name>`

16.	If you get a merge with conflicts: 
	1.	Go and fix the files (it can be in an IDE, e.g. VS Code or Eclipse, or another editor such as Notepad++) 
	2.	Then save the files  
	3.	Then run git status.  You will probably see a bunch of green entries (already added to stage because they were automatically merged).  But you will see the files that you manually fixed in red.
	4.	Run “`git add .`”, which will add the files you changed to stage.
	5.	Run `git status` again, and all files should be green.
	6.	Then run `git commit -m “<comment>”` and it will commit the automatically merged and manually fixed (merged) files.
	7.	`git status` should reveal a clean working directory
	8.	Then you may push the changes upstream
17.	To update branch from master:
	1.	`git checkout master`
	2.	`git pull`
	3.	`git checkout <branch name>`
	4.	`git merge master`
	5.	merge if necessary
	6.	test the code
	7.	commit merged changes
	8.	push merged changes
18.	How to create a branch from a tag:
	1.	First, query all the tags for the one you want (in this case, I’m looking for build 138):
	`git tag | grep 138`
	2.	Invoke `git branch <branch-name> <tag-name>`
	`git branch ibmsw-problem q2oNAJboss-Q2O_NA_JBOSS_DEV_Deploy-138`
	3.	Then checkout that new branch:
	`git checkout ibmsw-problem`

## SQL Queries
## Shell Scripting
### How to kill all java processes in korn shell  
`ps -ef | grep java | grep -v grep | awk '{print "kill " $2}' | ksh –x`  
### Here is a way to search for classes in a jar on local using cygwin  
`grep JamServiceFactory c:/axis2-1.4.1/lib/*.jar`  
Then you’ll get some results back like this:  
```
Binary file c:/axis2-1.4.1/lib/annogen-0.1.0.jar matches
Binary file c:/axis2-1.4.1/lib/xmlbeans-2.3.0.jar matches
```  
Then you can look to see if the exact class including package name exists in the jar:  
```
find c:/axis2-1.4.1/lib/ -name "annogen-0.1.0.jar" -exec jar tvf {} \; | grep -i JamServiceFactory
2427 Sat Dec 11 22:25:58 GMT-07:00 2004 org/codehaus/jam/JamServiceFactory.class
6800 Sat Dec 11 22:25:58 GMT-07:00 2004 org/codehaus/jam/provider/JamServiceFactoryImpl.class

find c:/axis2-1.4.1/lib/ -name "xmlbeans-2.3.0.jar" -exec jar tvf {} \; | grep -i JamServiceFactory
2675 Tue May 22 13:26:08 GMT-07:00 2007 org/apache/xmlbeans/impl/jam/JamServiceFactory.class
7400 Tue May 22 13:26:08 GMT-07:00 2007 org/apache/xmlbeans/impl/jam/provider/JamServiceFactoryImpl.class
```  
### To force remove a directory and all of its contents and children, use the following command:  
```
rm –rf <directory name>
```
### How to find out what process is writing to a file  
```
fuser {file}
```
For example:  
```
fuser /usr/WebSphere/AppServer/profiles/ProdNode1/velocity.log
```
### To run xwindows:  
1. Open cygwin
2. Issue command  `startx`
3. From Xwindows
	1. run command `xhost  +`
4. On the host where you are running the gui program, enter the following command:
	1. `export DISPLAY=172.16.131.141:0.0`  

In the above command, use your actual ip address.  IMPORTANT: The only exception to this is if you’re using X11 forwarding.  In that case do not export the display to your IP, otherwise the X11 forwarding will not work due to there being a firewall in between your PC and the server.    

To test this, issue the command `xclock` - this should display a graphical clock in a window.  
<strong>On external webservers:</strong>  

In the PuTTY configuration, make sure X11 forwarding is checked for the profile of the server you want to connect to as shown below:  

![PuTTY configuration](/putty-config.png)  
Make sure to save this setting so you don’t have to do it every time.  
Try the `xclock` command to test whether X11 is working or not.  If you `su` to another user, you must do the following, while in the home directory of the user you “su’d” to. `cp /home/<original userid>/.Xauthority .` < note the trailing ‘space dot’ >  

Next, `chmod 666 .Xauthority`  

Use `xclock` to test.  

### How to replace all occurrences of one string with another using VI
`:%s/oldstring/newstring/g`  

### Here is a way to clear a log file while an app is running:  
`cat /dev/null > [file name / path]`  

Here is an example:  

`cat /dev/null > /usr/WebSphere/AppServer/profiles/TestNode2/logs/q2o01b/native_stderr.log`  

This will result in the file size being set to zero and the file contents being thrown away!  
### Script to recursively replace one string with another
The script is currently in the /javadumps file system on london1.  Here are the contents of it:
```
#!/bin/sh
# first arg: File Pattern; e.g "*.xml"
# second arg: pattern to search for; e.g. "find string"
# third arg: replace string; e.g. "replace string"

if [ $# -lt 3 ] ; then
    echo -e "Wrong number of parameters."
    echo -e "Usage:"
    echo -e " renall 'filepat' findstring replacestring\n"
    exit 1
fi
#echo $1 $2 $3
for i in `find . -name "$1" -exec grep -l "$2" {} \;`
do
    mv "$i" "$i.sedsave"
    sed "s/$2/$3/g" "$i.sedsave" > "$i"
    echo $i
    #rm "$i.sedsave"
done
```
### Turning off / on the Highlighting on Linux VI
to turn off highlighting on a VI editor in Linux edit the ~/.viminfo file and change the highlighting param from `H` to `h`

Then if you want to turn it back on temporarily, issue:  

`:set hlsearch`  
### Set up SCP / SSH without password prompt:
While signed in on the “source” box as the user who will be doing the SSH or SCP run this command from the home directory for that user:
```
ssh-keygen -t rsa -f .ssh/id_rsa
```
Then it will create a file called ‘id_rsa.pub’.  

Then log onto the target box (that you will be SCPing or SSHing into) as the user you will be connecting as.  In the /home/username/.ssh directory see if there is already a file called “authorized_keys”.  If not, create one and put the contents of the id_rsa.pub created on the source box into it.  The contents may look something like this:
```
ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAIEA1XlwDreSZScOWkrupLjbJmGZD1szrnn6prKiIS4/QTPZxapkomBqjTIt07JZS0vrEwwDBzRXOnOmJGaGDzCcdhM8dmJ4QPkhyfRpHBVPRxAS8tsVHGIxJYo33SudY27rd2P/zaqV+0pVoUlZzu5snDvgmz34u3/oQNxXZ3k1XMs= wasadmin@cux120vig
```
If the file authorized_keys already exists, then just append to the end of it. Save the file.  

That’s it – you will be able to SSH or SCP without being challenged for a password.  

Note – if this doesn’t work, then it may be permissions related.  It didn’t work for me on AIX(source) -> Linux(target) one time and I ran this on the target Linux box:  
```
chmod 440 /home/wily/.ssh/authorized_keys
```
This is because what I found on this on a forum post:  

[http://www.unix.com/red-hat/12233-authorized_keys-passwordless-login.html](http://www.unix.com/red-hat/12233-authorized_keys-passwordless-login.html)  

**"Here's one thing to check....authorized_keys will be ignored if it is writable or could be replaced by anyone other than you. So $HOME must be writable only by you. Ditto $HOME/.ssh and $HOME/.shh/authorized_keys. This also applies to the parent directory of $HOME all the way back to /."**

## MongoDB
## Java
### Intellij IDEA Shortcuts
### Maven
### Spring
#### REST
##### Swagger Code Gen Steps
1. Clone git repo for the swagger-codegen tool into some directory:
`git clone https://github.com/swagger-api/swagger-codegen.git`
2. Run maven to build it:
`mvn clean install`
3. Run the following command to generate the sources:
`java -DdebugModels -jar modules/swagger-codegen-cli/target/swagger-codegen-cli.jar generate --library jersey1 -i swagger-file.json -l java -o generated-source`
4. Note the following things about this command
	1. -DdebugModels – it can be taken out, just for trying to troubleshoot this tool
	2. The goal is generate
	3. --library jersey1 – is critical – this causes the source code to be generated using JSON annotations rather than GSON
	4. -i getSAP46CostPrice-swagger-file.json – this is the JSON schema file
		1. Click on API Gallery on the top tabs
		2. Click on “View Details >>” button under “getSAP46CostPrice”
		3. Click on “Export API as” and select “JSON” and click export and you will get a JSON schema downloaded.  That is the file referred to in the command with -i
	5.-l java – this is “L” in lower case – represents the language.
	6. -o generated-source – this is the output directory
5. Then import existing maven project into eclipse and select the ‘generated-source’ directory as the root directory.
6. Fix any compile errors 
7. Copy in the JSON schema file to the project
8. then export the jar selecting only the io.swagger.client.model directory and the JSON schema file
9. Upload that file to the Nexus repository with the appropriate version number - here is a sample command:
`mvn deploy:deploy-file -Dfile=<jar file path/name> -DrepositoryId=nexus -Durl=http://my.nexus.repo:8080/repository/maven-releases -Dversion=4.0.0 -DgroupId=q2o.46 -DartifactId=ws-my-service`
10. Make the POM and code updates in the codebase.
### JPA-Hibernate
Here is how to create an "in" query using CriteriaBuilder:
```
var in = cb.in(tableRoot.get("colName"));  
colNames.forEach(in::value);  
cq.where(in);  
```
## Other
