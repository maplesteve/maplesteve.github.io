---
layout: post
title: Jenkins PMD analysis for Objective-C with oclint
category: digital
tags: jenkins howto dev oclint
related: ["Create Jira issues from Jenkins;/2013/03/24/create-jira-issues-from-jenkins", "Use headerdoc on Ubuntu;/2012/02/26/install-headerdoc-on-ubuntu"]
---

[OCLint](http://oclint.org) is a static code analyzer for C, C++ and Objective-C. You'll find it [here](https://github.com/oclint/oclint) on GitHub.

Today the maintainer merged my pull request in which I made an additional reporter module which writes a PMD-style file.

Having that, you can let the PMD Analysis run by Jenkins reports about your coding sins.

![](/media/oclint-pmd-report_500.png)

Here's how to set the whole thing up:

##### 1. Setup OCLint
* Get the latest source code from GitHub
* Follow the [instructions](http://docs.oclint.org/en/dev/intro/build.html) to build OCLint
* [Install](http://docs.oclint.org/en/dev/intro/installation.html) it.

##### 2. Build file for Jenkins
The invocation of OCLint is configured in a build.xml. I'll walk you through this one.

	<?xml version="1.0" encoding="UTF-8"?>
	<project name="fooProject" default="build-fooProject">

	<property environment="env"/>

	<target name="build-fooProject" depends="prepare,oclint" />
	
	<target name="clean" description="Cleanup build artifacts">
		<delete dir="${basedir}/build/oclint" /> 
	</target>
	
	<target name="prepare" depends="clean" description="Prepare for build">
		<mkdir dir="${basedir}/build/oclint" /> 
	</target>

Standard stuff so far. Setup the project and prepare the directories.

	<target name="oclint">
		<antcall target="xcodebuild-clean" />
		<antcall target="xcodebuild" />
		<antcall target="oclint-xcodebuild" />
		<antcall target="oclint-parse" />
	</target>

Our `oclint` invocation has four steps.

	<target name="xcodebuild-clean">
		<exec executable="xcodebuild">
			<arg value="-configuration" />
			<arg value="Release" />
			<arg value="clean" />
		</exec>
	</target>
 This ensures, that we have a clean build.
 
	<target name="xcodebuild">
		<exec executable="xcodebuild" output="xcodebuild.log">
			<arg value="-configuration" />
			<arg value="Release" />
			<arg value="-arch" />
			<arg value="armv7" />
			<arg value="CODE_SIGN_IDENTITY=" />
			<arg value="CODE_SIGNING_REQUIRED=NO" />
		</exec>
	</target>
Now we build our project. The important part is `output="xcodebuild.log"`; this will write the output to a file which will be fed to a helper script in the next step.

	<target name="oclint-xcodebuild">
		<exec executable="PATH_TO_oclint-release/bin/oclint-xcodebuild" />
	</target>
	
`oclint-xcodebuild` reads the `xcodebuild.log` and produces the file `compile_commands.json`. This file holds all the compiler stuff and is the input format for `oclint`.

	<target name="oclint-parse">
		<exec executable="PATH_TO_oclint-release/bin/oclint-json-compilation-database">
			<env key="PATH" value="${env.PATH}:PATH_TO_oclint-release/bin/"/>
		 	<arg value="--" />
		 	<arg value="-o=${basedir}/build/oclint/lint.xml" />
		 	<arg value="-report-type=pmd" />
		 	<arg value="-stats" />
		</exec>
	</target>
	</project>
Finally, this is where the magic happens. `oclint-json-compilation-database` feeds the `compile_commands.json` file to `oclint`. The `-report-type=pmd` flag tells it to use the PMDReporter, which will write its findings to a file called `lint.xml`.

Be sure to consult the documentation for OCLint and its helpers for the various arguments you can provide.

I created a gist with the whole file [here](https://gist.github.com/maplesteve/5129782).

##### 3. Configure the job in Jenkins
* Go to the configuration page of your job in Jenkins.
* Add a build-step with the build.xml
* Add a post-build action "Publish PMD analysis results" and enter the path to the xml file we produced. In this example it would be `build/oclint/lint.xml`


##### 4. Build the job
If everything worked, you should have a new section "PMD Warnings" in your build information and after a few builds the trend chart will be produced.