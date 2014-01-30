# Ingredients for Success (What you should do and the tools you should consider)




## Installing and configuring the environment is automated in the code, rather than a manual process run by a systems group.

A key idea (maybe *the* key idea) of devops is that the environment in which your code will run should be modeled as code, and not be some separate thing that is a black box.  (And, as we get a bit further down the stack, should be versioned with the code, as well.)  It seems pretty basic, but the idea is that you should have a "recipe" that allows you to recreate the environment at any moment.  Some of the key parts of managing the environment include:

* *General configuration*.  General configuration includes setting up the basic requirements for the app to even run -- things like ensuring that whatever directory it will live in actually exists, creating the user a service will run as, specifying where log files should be stored, exposing (or blocking) the proper ports, setting any required permissions, installing any license or cert files, updating packages, and on and on.  In short, anything that an app needs at the basic operating system level.  
* *Installation and configuration of the required backing services*.  The [12 Factor App](http://12factor.net) describes [backing services](http://12factor.net/backing-services) as "any service the app consumes over the network as part of its normal operation. Examples include datastores (such as MySQL or CouchDB), messaging/queueing systems (such as RabbitMQ or Beanstalkd), SMTP services for outbound email (such as Postfix), and caching systems (such as Memcached)."  Backing services can also include 3rd party services, like Amazon AWS (SQS, dynamodb etc),  [GitHub](http://developer.github.com/v3/), [Twitter](https://dev.twitter.com/), [Parse](https://www.parse.com/), and a host of other services.  Ideally, the "code for a twelve-factor app makes no distinction between local and third party services."
* *Installation of the development environment itself*.  If you're writing a Rails app, for example, you'll need to have the required versions of Ruby, Rails, bundler, and anything else you need.  The same goes for any other stack.

Some examples of tools to include here:

* chef
* puppet
* ansible
* salt
* dockerfile


## A local development environment that closely matches production.

One of the key breakthroughs of the devops idea is giving developers a simple way to install and run the entire app on their local machine.  Being able to run it on their own system encourages creativity and flexibility, and make development much more fun and productive.  

[Vagrant](http://www.vagrantup.com/) is the key tool here -- basically, it takes the recipes you created with your environment tool (i.e., your chef or puppet files) and _provisions_ (creates) a virtual machine that runs in a tool like [Virtualbox](https://www.virtualbox.org/) or [VMWare](http://www.vmware.com/).  Vagrant automatically maps a virtual drive from the virtual instance back to the host machine, allowing the developer to use his or her favorite editor / IDE, but still run the application in an environment that matches the production environment as closely as possible.

Key tools here are:

* Vagrant 

Something about mocking up 3rd party services (APIs, etc) on a local machine:

* [canned](https://github.com/sideshowcoder/canned)

## Code is stored in a distributed version control system (probably git) with hooks to external services.

The version control system is the heart of the new development process.  At the most basic level, a VCS allows developers to keep track of all the changes made to a set of files and be able to roll back to specific points in time in case something screws up.  In some systems, like [Subversion](http://subversion.apache.org/), the code is checked out and then checked back in from a central repository. If there is a conflict between two developers' files (for example, both of them edited the same line of code), then the two version must be merged together.  This can be a painful process. 

In contrast, distributed version control systems (DVCS), like [git](http://git-scm.com/), are the heart of most new style development processes. Rather than having a central, master copy that makes it difficult and expensive to merge a lot of contributions from developers, a DVCS makes it simple (well, simpler!) to have multiple people all working on the same codebase simultaneously in different _branches_, and that these branches can be easily merged back together in a _master_ branch.  

While there are many different work styles, such as [git flow](http://nvie.com/posts/a-successful-git-branching-model/), the basic DVCS process is:

* there is an agreed upon master repository, which is often on a public service like [GitHub](https://github.com/), [BitBuckt](https://bitbucket.org/) or an internal server like [GitLab](https://www.gitlab.com/) or [Mercurial](http://mercurial.selenic.com/)
* each developer clones the master repository to his or her local machine
* the developer creates a new branch, usually for a specific feature
* the developer makes commits against the local copy
* once the feature is done, he or she merges the branch back into the master branch and pushes the change back to the master
* other developers pull from the and merge their branch
* the merged copy preserves the full version history of all the distributed copies

In addition to these coordination functions, most version control system also offer a feature called a _hook_.  A hook is a process that fires once a specific event, like a commit, happens to the repository.  Hooks can be defined in the repo itself, but also in the hosting service.  For example, GitHub lets you define "service" hooks that are called whenever a specific event occurs.  These hooks are the tie-in to the continuous integration (CI server).


## Changes in the master branch of the codebase trigger a continuous integration server to deploys code or perform tests.

The CI server's executes a specific action on a repository whenever it receives a commit hook.  For example, is a developer makes a commit against repository called foo, the CI server might:

* clone down a local copy of foo
* execute foo's test suites (see the section on application stacks for more about this)
* if the tests fail, the CI server sends an alert to the development team and halts the process
* if the test suite passes, the CI server might deploy the code to a staging or even production server


Some key examples of CI servers include:


* Jenkins
* Travis
* others...

## The cloud is the default deployment platform.



## All servers and important system are instrumented and monitored from a central service.


* Nagios
* Scout

* New Relic



* Pagerduty


* Chatops





