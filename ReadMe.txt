 _____  _____  _____  ____   _____  _____    _____  _____  _____ 
| __  ||   __||  _  ||    \ |     ||   __|  |   __||     || __  |
|    -||   __||     ||  |  || | | ||   __|  |   __||  |  ||    -|
|__|__||_____||__|__||____/ |_|_|_||_____|  |__|   |_____||__|__|
                                                                 
 ____   _____  _____  __     _____  __ __  _____  _____  _____  _____ 
|    \ |   __||  _  ||  |   |     ||  |  ||     ||   __||   | ||_   _|
|  |  ||   __||   __||  |__ |  |  ||_   _|| | | ||   __|| | | |  | |  
|____/ |_____||__|   |_____||_____|  |_|  |_|_|_||_____||_|___|  |_|  
                                                                        
 _____  _____    _____  _____  _____    _____  _____  _____  __     _____ 
|     ||   __|  |     ||  |  || __  |  |_   _||     ||     ||  |   |   __|
|  |  ||   __|  |  |  ||  |  ||    -|    | |  |  |  ||  |  ||  |__ |__   |
|_____||__|     |_____||_____||__|__|    |_|  |_____||_____||_____||_____|
                                                                          

			Table of contents:
			1. Project workspace structure
			2. Prerequisites
			3. Building and deploying
			4. More information


1. PROJECT WORKSPACE STRUCTURE

All tools are grouped in this project's pom.xml-file. All commands in this
readme must be executed in this project's base directory. This project's
directory is located next to the child projects in the same directory. The file
structure should look like the following:

- Tools
  - pom.xml
- TOVAL
  - src/
  - res/
  - pom.xml
- JAGAL
  - src/
  - pom.xml
- SEWOL
  - src/
  - ext/
  - pom.xml
- SEPIA
  - src/
  - pom.xml


2. PREREQUISITES

A good introduction to Apache Maven can be found under
https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html

To be able to deploy the tools to the repository, some changes in the Maven
settings are needed. Under Ubuntu the settings can be found under the following
path: /etc/maven/settings.xml.

To be allowed to upload artifacts to the OSSRH, a Jira user account under
  https://issues.sonatype.org/secure/Dashboard.jspa
is needed and the user must have permissions in the group
  de.uni.freiburg.iig.telematik.
The user credentials must be added to the "servers" block in the settings:

	<server>
	  <id>ossrh</id>
	  <username>USERNAME</username>
	  <password>PASSWORD</password>
	</server>

To sign the generated artifacts, Apache Maven needs the PGP passphrase. It can
be added in the section "profiles", where a new profile must be added. The
"PGP_PASSPHRASE" must be replaced by the actual passphrase.

	<profile>
	  <id>ossrh</id>
	  <activation>
	    <activeByDefault>true</activeByDefault>
	  </activation>
	  <properties>
	    <gpg.executable>gpg2</gpg.executable>
	    <gpg.passphrase>PGP_PASSPHRASE</gpg.passphrase>
	  </properties>
	</profile>


3. BUILDING AND DEPLOYING

All of them can be built and locally installed at once with the following
command:

  $ mvn clean install

For a release, the correct version number must be set. Snapshot releases can be
created arbitrarily and their versions always end with "-SNAPSHOT". To make a
normal release, you can remove the "-SNAPSHOT" suffix. The version number can
be set in all sub-projects with their dependencies using the following command:

  $ mvn versions:set -DnewVersion=0.1.6-SNAPSHOT

For the releasing deployment, the profile "release" must be called:

  $ mvn clean deploy -P release

The resulting snapshots can be found under:

- https://oss.sonatype.org/content/repositories/snapshots/de/uni/freiburg/iig/


4. MORE INFORMATION

More information can be found under:
- http://central.sonatype.org/pages/ossrh-guide.html
- http://central.sonatype.org/pages/apache-maven.html
- https://maven.apache.org/guides/mini/guide-central-repository-upload.html