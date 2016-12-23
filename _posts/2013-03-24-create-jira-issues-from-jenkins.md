---
layout: post
title: Jenkins Plugin&#58; Create Jira issues for failed unit tests with JiraTestResultReporter
category: digital
tags: howto dev jenkins
related: ["PMD analysis for Objective-C with Jenkins and OCLint;/2013/03/10/jenkins-pmd-analysis-for-objective-c-with-oclint", ",Use headerdoc on Ubuntu;/2012/02/26/install-headerdoc-on-ubuntu"]
---
**Update: The pluginis now available in the official Jenkins plugin repository!**
**Details here: <https://wiki.jenkins-ci.org/display/JENKINS/JiraTestResultReporter-plugin>**

---

**Testing your code with unit tests is a fine thing and using a [Jenkins CI server][3] for those tests is even better. Automatically creating issues in Jira for failed tests makes the workflow complete. This is what the JiraTestResultReporter plugin for Jenkins does.**

This plugin examines the build job for failed unit tests. It work by using the Jenkins internal test result management for detecting failed tests. Just let Jenkins run and report your unit tests e.g. by adding the "Publish xUnit test results report" to your build job.

If JiraTestResultReporter detects *new* failed tests, it will create an issue for every test case in Jira:

![](/media/jirascreen_500.png)

### Installation

As long as my hosting request to get the plugin included in the official plugin repository of Jenkins CI is pending, you'll have to either build the plugin yourself or you can download the recent snapshot:

* Build yourself
	* Download or clone the source code from [GitHub][1]
	* ```cd``` into the downloaded directory
	* execute the maven command ```mvn package```

or

* Download the plugin package from [here][2].

then

* Upload the built or downloaded file ```JiraTestResultReporter.hpi``` to the plugins directory of your Jenkins installation or use the plugin uploader from *Manage Jenkins* -> *Manage Plugins* -> "Advanced" tab
* restart Jenkins

### Usage
* In the build job add JiraTestResultReporter as a post-build action.
* Configure the plugin for this job. See the help boxes for details. I have the dedicated Jira user 'jenkins_reporter' for these kinds of automatic reports.

![](/media/configscreen_500.png)

* Build your job. If there are failed tests, the plugin will create issues for them. This will (should!) happen only once for every new failed tests; new in this case means tests that have an age of exactly 1.

[1]: https://github.com/maplesteve/JiraTestResultReporter        "JiraTestResultReporter on GitHub"
[2]: http://maplesteve.com/media/2013/03/JiraTestResultReporter.hpi        "JiraTestResultReporter.hpi"
[3]: https://jenkins-ci.org        "Jenkins CI"
