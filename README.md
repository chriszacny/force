## force

A command-line interface to force.com
![](https://travis-ci.org/devangel/force.svg?branch=master)

![](https://travis-ci.org/devangel/force.svg?branch=master)

### Installation

##### Precompiled Binaries

* [Linux 32bit](https://godist-new.herokuapp.com/projects/heroku/force/releases/current/linux-386/force)
* [Linux 64bit](https://godist-new.herokuapp.com/projects/heroku/force/releases/current/linux-amd64/force)
* [Linux Arm](https://godist-new.herokuapp.com/projects/heroku/force/releases/current/linux-arm/force)
* [OS X 32bit](https://godist-new.herokuapp.com/projects/heroku/force/releases/current/darwin-386/force)
* [OS X 64bit](https://godist-new.herokuapp.com/projects/heroku/force/releases/current/darwin-amd64/force)
* [Windows 32bit](https://godist-new.herokuapp.com/projects/heroku/force/releases/current/windows-386/force.exe)
* [Windows 64bit](https://godist-new.herokuapp.com/projects/heroku/force/releases/current/windows-amd64/force.exe)

##### Compile from Source

    $ go get -u github.com/heroku/force

### Usage

	Usage: force <command> [<args>]
	
	Available commands:
	   login     force login [-i=<instance>] [<-u=username> <-p=password>]
	   logout    Log out from force.com
	   logins    List force.com logins used
	   active    Show or set the active force.com account
	   whoami    Show information about the active account
	   describe  Describe the object or list of available objects
	   sobject   Manage standard & custom objects
	   field     Manage sobject fields
	   record    Create, modify, or view records
	   bulk      Load csv file use Bulk API
	   fetch     Export specified artifact(s) to a local directory
	   import    Import metadata from a local directory
	   export    Export metadata to a local directory
	   query     Execute a SOQL statement
	   apex      Execute anonymous Apex code
	   oauth     Manage ConnectedApp credentials
	   test      Run apex tests
	   security  Displays the OLS and FLS for a give SObject
	   version   Display current version
	   update    Update to the latest version
	   help      Show this help
	   push      Deploy artifact from a local directory
	   aura      force aura push -resourcepath=<filepath>
	   password  See password status or reset password
	   notify    Should notifications be used
	   limits    Display current limits
	
	Run 'force help [command]' for details.



### login
When you login using the CLI a record of the login is saved. Eventually your token will expire requiring re-authentication. The default login is for all production instances of salesforce.com. Two predefined non-production instances are available using the test and pre aliases.  You can set an arbitrary instance to log in to by specifying the instance url in the form of subdomain.domain. For example login-blitz.soma.salesforce.com.

      force login               # log in to production or developer org
      force login -i=test           # log in to sandbox org
      force login -i=pre            # log in to prerelease org
      force login -u=un -p=pw       # log in using SOAP
      force login -i=test -u=un -p=pw       # log in using SOAP to sandbox org
      force login -i=<instance> -u=un -p=pw     # internal only

### logout
Logout will delete your authentication token and remove the saved record of that login.

      force logout -u=user@example.org

### logins
Logins will list all the user names that you have used to authenticate with the instance URL associated with each one.  The active login will be indicated behind the login name in red.

      force logins

![](https://raw.githubusercontent.com/dcarroll/dcarroll.github.io/master/images/force/screenshot-191.png)

### active
Active without any arguments will display the currently active login that you are using. You can also supply a username argument that will set the active login to the one corresponding to the username argument. Note, just because you set a login as active, does not mean that the token is necessarily valid.

      force active
      force active dave@demo.1

### whoami
Whoami will display detailed user information about the currently active logged in user.  This is Force.com specific information.

      force whomai

![](https://raw.githubusercontent.com/dcarroll/dcarroll.github.io/master/images/force/screenshot-191%20copy.png)

### describe
	Usage: force describe (metadata|sobject) [-n=<name> -json]
	
	  -n, -name       # name of specific metadata to retrieve
	  -json           # output in JSON format
	
	  Examples
	
	  force describe metadata -n=CustomObject
	  force describe sobject -n=Account
	  force describe metata
	  force describe sobject



### sobject
Sobject command gives you access to creating and deleting schema objects. The list argument will list ALL of the objects, both standard and custom, in your org.

      force sobject list
      force sobject create <object> [<field>:<type>]...
      force sobject delete <object>

![](https://raw.githubusercontent.com/dcarroll/dcarroll.github.io/master/images/force/screenshot-192.png)

### field
Field gives you the ability to create, list and delete the fields on an object. Fields need to be created one at a time. You can also set required and optional attributes for the type of field. All defaultable field attributes will be defaulted based on the defaults in the web UI.

      force field list Todo__c
      force field create Todo__c Due:DateTime required:true
      force field delete Todo__c Due

### bulk
	Usage: force bulk insert Account [csv file]
	
	Load csv file use Bulk API
	
	Examples:
	
	  force bulk insert Account [csv file]
	
	  force bulk update Account [csv file]
	
	  force bulk job [job id]
	
	  force bulk batches [job id]
	
	  force bulk batch [job id] [batch id]
	
	  force bulk batch retrieve [job id] [batch id]
	
	  force bulk query Account [SOQL]
	
	  force bulk query retrieve [job id] [batch id]


### fetch
	Usage: force fetch -t ApexClass
	
	  -t, -type       # type of metadata to retrieve
	  -n, -name       # name of specific metadata to retrieve (must be used with -type)
	  -d, -directory  # override the default target directory
	  -u, -unpack     # unpack any zipped static resources (ignored if type is not StaticResource)
		-p, -preserve   # preserve the zip file
	
	Export specified artifact(s) to a local directory. Use "package" type to retrieve an unmanaged package.
	
	Examples
	
	  force fetch -t=CustomObject n=Book__c n=Author__c
	  force fetch -t Aura -n MyComponent -d /Users/me/Documents/Project/home


### import
	Usage: force import [deployment options]
	
	Import metadata from a local directory
	
	Deployment Options
	  -rollbackonerror, -r    Indicates whether any failure causes a complete rollback
	  -runalltests, -t        If set all Apex tests defined in the organization are run
	  -checkonly, -c          Indicates whether classes and triggers are saved during deployment
	  -purgeondelete, -p      If set the deleted components are not stored in recycle bin
	  -allowmissingfiles, -m  Specifies whether a deploy succeeds even if files missing
	  -autoupdatepackage, -u  Auto add files to the package if missing
	  -ignorewarnings, -i     Indicates if warnings should fail deployment or not
	  -directory, -d 		  Path to the package.xml file to import
	  -verbose, -v 			  Provide detailed feedback on operation
	
	Examples:
	
	  force import
	
	  force import -directory=my_metadata -c -r -v
	
	  force import -checkonly -runalltests


### export
	Usage: force export [dir]
	
	Export metadata to a local directory
	
	Examples:
	
	  force export
	
	  force export org/schema


### query
	Usage: force query <soql statement> [output format]
	
	Execute a SOQL statement
	
	Examples:
	
	  force query select Id, Name, Account.Name From Contact
	
	  force query select Id, Name, Account.Name From Contact --format:csv


### apex
	Usage: force apex [file]
	
	Execute anonymous Apex code
	
	Examples:
	
	  force apex ~/test.apex
	
	  force apex
	  >> Start typing Apex code; press CTRL-D when finished

### oauth
	Usage: force oauth <command> [<args>]
	
	Manage ConnectedApp credentials
	
	Usage:
	
	  force oauth create <name> <callback>
	
	Examples:
	
	  force oauth create MyApp https://myapp.herokuapp.com/auth/callback


### test
	Usage: force test (all | classname...)
	
	Run apex tests
	
	Test Options
	  -namespace=<namespace>     Select namespace to run test from
	
	Examples:
	
	  force test all
	  force test Test1 Test2 Test3
	  force test -namespace=ns Test4 


### security
	Usage: force security [SObject]
	
	Displays the OLS and FLS for a given SObject
	
	Examples:
	
	  force security Case


### version
	Usage: force version
	
	Display current version
	
	Examples:
	
	  force version


### update
	Usage: force update
	
	Update to the latest version
	
	Examples:
	
		force update


### push
Push gives you the ability to push specified resources to force.com.  The resource will be pulled from ./src/{type}/

      force push -t(ype) StaticReource -n(ame) MyResource.resource
	  force -type ApexClass -name MyClass.cls
	  force -t ApexPage -n MyPage.page

You can also push all of a specific type of resource from a given folder.

      force push -t StaticResource -p(ath) src/staticresources/
      force push -t ApexClass -path src/classes/
      force push -t ApexPage -p src/pages/

### aura
	Usage: force aura

	The aura command needs context to work. If you execute "aura get"
	it will create a folder structure that provides the context for
	aura components on disk.

	The aura components will be created in "metadata/aurabundles/<componentname>"
	relative to the current working directory and a .manifest file will be
	created that associates components and their artifacts with their ids in
	the database.

	To create a new component (application, evt or component), create a new
	folder under "aura". Then create a new file in your new folder. You
	must follow a naming convention for your files to enable proper definition
	of the component type.

	Naming convention <compnentName><artifact type>.<file type extension>
	Examples: 	metadata
					aura
						MyApp
							MyAppApplication.app
							MyAppStyle.css
						MyList
							MyComponent.cmp
							MyComponentHelper.js
							MyComponentStyle.css

	force aura push -f <fullFilePath> -b <bundle name>

	force aura create -t=<entity type> <entityName>

	force aura delete -f=<fullFilePath>

	force aura list


### password
	Usage: force password <command> [user name] [new password]
	
	See password status or reset/change password
	
	Examples:
	
	  force password status joe@org.com
	
	  force password reset joe@org.com
	
	  force password change joe@org.com $uP3r$3cure

### notify
Includes notification library, [gotifier](https://github.com/ViViDboarder/gotifier), that will display notifications for using either Using [terminal-notifier](https://github.com/alloy/terminal-notifier) on OSX or [notify-send](http://manpages.ubuntu.com/manpages/saucy/man1/notify-send.1.html) on Ubuntu. Currently, only the `push` and `test` methods are displaying notifications.

### limits
Limits will display limits information for your organization.
- Max is the limit total for the organization
- Remaining is the total number of calls or events left for the organization

The list is limited to those exposed by the REST API.
	
      force limits

### Hacking

    # set these environment variables in your startup scripts
    export GOPATH=~/go
    export PATH="$GOPATH/bin:$PATH"

    # download the source and all dependencies
    $ go get -u github.com/heroku/force
    $ cd $GOPATH/src/github.com/heroku/force

    # to compile and test modifications
    $ go get .
    $ force
