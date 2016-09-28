# DevOps-Project

###Team:
* Anand Varma Chekuri (ACHEKUR)
* Shrey Sanghavi (SSANGHA)

--

<br/>

### Build
---

#### The Setup Process

*Installing Jenkins:*

	sudo yum -y install java
	sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat/jenkins.repo
	sudo rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key
	sudo yum install jenkins
	sudo systemctl start jenkins.service
	
*Installing Dependencies:*
	
	sudo yum install -y git
	sudo yum install -y maven
	sudo yum install java-1.6.0-openjdk-devel
	sudo yum install mailx
	
	
*The project (gson), build-management (maven)* **TODO!**

--


<br/>
#### 1. Trigger Builds on Commit

In order to trigger a build every time a commit is pushed to the GIT repo, we use Github Webhooks. The webhook is configured to send out a POST request to Jenkins' GIT plugin every time **push** event is observed by the GIT server.

![Github WebHook configuration](images/github-webhook.png)

Also, while configuring a project on Jenkins, we check the "Build when a change is pushed to GitHub" options. This completes the trigger configuration from the Jenkins' side and the GIT plugin handles the POST requests made by the webhook.

![Github WebHook configuration](images/jenkins-trigger.png)


--

<br/>
#### 2. Clean Builds, Build Management

To ensure that each build happens in a clean and controlled environment, we configured Jenkins to delete the workspace before a new build starts. This prevents running build jobs with stale artifacts from a previous builds.

![Clean workspace before building - Jenkins Config](images/clean-builds.png)

Also, the build script is written to execute **"mvn clean"** before building. This way, we can be sure that the build environment is always new.

![Clean workspace before building - Jenkins Config](images/build-sh.png)


--

<br/>
#### 3. Handling build success/failures
Jenkins comes pre-installed with a plugin that sends out emails to a given set of addresses when a critical event occurs *i.e.* build-fails or when an unstable project becomes stable, etc.

In order to get finer control over what actions need to taken when a build Passes or Fails, we installed and configured the [Hudson Post Build Task](http://wiki.hudson-ci.org/display/HUDSON/Post+build+task) plugin. This plugin allows us to configure custom shell scripts to run upon job completion, based on the return value (PASSED or FAILED). We used the following script to send out an email to a particular recepient every time a build passes, in-order to demonstrate post-build script functionality.

	echo "Build was successful" | mail -s "[Jenkins][GSON Fork] Build Success" ssangha@ncsu.edu


--

<br/>
#### 4. Branch specific builds

Lorem Ipsum


--

<br/>
#### 5. Build Hisory

Jenkins comes pre-loaded with the option to browse through the "Build History" for all the configured projects. This view is presented in HTML and can be accessed via a web-browser at the following link: [http://JENKINS_IP:8080/view/All/builds](http://JENKINS_IP:8080/view/All/builds)