# Getting Started

Welcome to Reference Implementation, the new Content Delivery web application! This tutorial will take you through creating a custom application from scratch.
We assume you are familiar with **TypeScript, JavaScript (ES6), Webpack, HTML, XML, React, LESS, CSS, Java, Spring, DXA and Maven**.

Some learning material:

1. [JavaScript (ES6)](https://egghead.io/courses/learn-es6-ecmascript-2015)
2. [TypeScript Getting Started](https://egghead.io/courses/up-and-running-with-typescript)
3. [TypeScript Tutorial](https://www.typescriptlang.org/docs/tutorial.html )
4. [React Fundamentals](https://egghead.io/courses/react-fundamentals)
5. [React Hello World](https://facebook.github.io/react/docs/hello-world.html)
6. [LESS](http://lesscss.org/)
7. [Webpack](https://webpack.js.org/)
8. [Webpack Course](https://egghead.io/courses/using-webpack-for-production-javascript-applications)
9. [Serving Web Content with Spring MVC](https://spring.io/guides/gs/serving-web-content/)
10. [Maven Getting Started Guide](https://maven.apache.org/guides/getting-started/)
11. [Getting started with DXA 1.7](http://docs.sdl.com/LiveContent/content/en-US/SDL%20DXA-v7/GUID-D36601FA-88DD-48A6-A8C0-61159673C2F4)
12. [Developing on DXA Java](http://docs.sdl.com/LiveContent/content/en-US/SDL%20DXA-v7/GUID-BEDE64AE-1E23-4784-AE5E-7DA84B26F1AA)

## Software requirements

Make sure you have installed:

* [Node.js](https://nodejs.org/)
  * 64 bit version is recommended
  * Install NodeJs v6.x or higher
* [Maven 3](https://maven.apache.org/download.cgi)
* [Tomcat 8.5](http://tomcat.apache.org/download-80.cgi)
* An IDE which has support for TypeScript / LESS
  * [Atom](https://atom.io/)
  * [Sublime Text](https://www.sublimetext.com/)
  * [Visual Studio Code](https://code.visualstudio.com/)
  * ...
* An IDE which has support for Java
  * [IntelliJ IDEA](https://www.jetbrains.com/idea/specials/idea/idea.html)
  * ...
* A Content Delivery environment with data

In the following guide we're using IntelliJ IDEA Ultimate (for Java) and Visual Studio Code (for LESS / TypeScript / JavaScript).

> You can find the result of this guide [here](../custom-webapp/)

## Setting up Maven

In order to download some snaphot artifacts which are used by the archetype. We will need to add a reference to the sonatype snaphots repository.

1. Open up your main `settings.xml` file. On an Windows machine this file is located at `C:\Users\<username>\.m2\settings.xml`, on a Linux Ubuntu it is at `/usr/share/maven/conf/settings.xml`.
2. Add an extra profile inside the `profiles` section (see code snippet below)
3. Make this the active profile (see code snippet below)

```xml
<settings>
    <profiles>
        <profile>
            <id>sonatype-nexus-snapshots</id>
            <repositories>
                <repository>
                    <id>sonatype-nexus-snapshots</id>
                    <name>sonatype-nexus-snapshots</name>
                    <url>https://oss.sonatype.org/content/repositories/snapshots</url>
                    <releases>
                        <enabled>false</enabled>
                    </releases>
                    <snapshots>
                        <enabled>true</enabled>
                    </snapshots>
                </repository>
            </repositories>
        </profile>
    </profiles>
    <activeProfiles>
        <activeProfile>sonatype-nexus-snapshots</activeProfile>
    </activeProfiles>
</settings>
```

## Create a new project using the Maven Archetype

First step you will need to do is to create a new directory.
We've created the following directory on our machine: `D:\github\dd-webapp-custom-examples`.
We'll be usign this directory further on in the examples.

Ok Let's start by opening the command prompt and run:

```bash
cd "D:\github\dd-webapp-custom-examples"
mvn archetype:generate "-DarchetypeArtifactId=dd-webapp-archetype" "-DarchetypeGroupId=com.sdl.delivery.ish" "-DarchetypeVersion=1.2.0"
```

Notice that we are using version `1.2.0`, this is just an example. We advise you to use the latest available version.
We'll pick `org.company` as our group ID and `custom-webapp` as the name of our artifact ID.

```bash
[INFO] Scanning for projects...
[INFO]
[INFO] ------------------------------------------------------------------------
[INFO] Building Maven Stub Project (No POM) 1
[INFO] ------------------------------------------------------------------------
[INFO]
[INFO] >>> maven-archetype-plugin:3.0.1:generate (default-cli) > generate-sources @ standalone-pom >>>
[INFO]
[INFO] <<< maven-archetype-plugin:3.0.1:generate (default-cli) < generate-sources @ standalone-pom <<<
[INFO]
[INFO] --- maven-archetype-plugin:3.0.1:generate (default-cli) @ standalone-pom ---
[INFO] Generating project in Interactive mode
[INFO] Archetype repository not defined. Using the one from [com.sdl.delivery.ish:dd-webapp-archetype:1.2.0] found in catalog remote
Define value for property 'groupId': org.company
Define value for property 'artifactId': custom-webapp
Define value for property 'version' 1.0-SNAPSHOT: :
Define value for property 'package' org.company: :
Confirm properties configuration:
groupId: org.company
artifactId: custom-webapp
version: 1.0-SNAPSHOT
package: org.company
 Y: : y
```

## Building the project

To buid the project, run the following command from the command line:

```bash
cd "D:\github\dd-webapp-custom-examples\custom-webapp"
mvn clean install
```

## Setting up IntelliJ IDEA development environment

Next step, we set up IntelliJ IDEA for development:

1. Launch IntelliJ
2. Open the directory where you've created the archetype, in our case this is the `D:\github\dd-webapp-custom-examples\custom-webapp` directory

### Setting up Content Delivery configuration

First step is to add the necessary configuration inside the `cd_client_config.xml` file, located at `src/main/resources/cd_client_conf.xml`. We need to provide the location of the discovery service, the client ID and the client secret of the Content Delivery system we are using.


![Setting up Content Delivery configuration](./images/cd-configuration.png)

### Setting up Tomcat

We'll describe this for usage with the IntelliJ Ultimate Edition and the IntelliJ Community Edition (free).
You can find a comparison of both versions [here](https://www.jetbrains.com/idea/features/editions_comparison_matrix.html).

After this you are ready to start [customizing the application](../README.md).

#### IntelliJ Community Edition

There is no tight integration with Tomcat and IntelliJ Community Edition, therefore some manual actions are required.

Install Tomcat plugin:

1. Download [Tomcat runner plugin for IntelliJ](https://plugins.jetbrains.com/plugin/8266-tomcat-runner-plugin-for-intellij)
2. Extract zip file inside plugins directory of IntelliJ installation (eg `C:\Program Files\JetBrains\IntelliJ IDEA Community Edition 2017.1.2\plugins`)
3. Restart IntelliJ

Start debugging:

1. Start IntelliJ as an Administrator
2. Click on the `Run` item in the top menu
3. Open `Edit Configurations`
4. Click on the `+` icon on the top left
5. Click on `Tomcat Runner`

![Add Tomcat Runner](./images/add-tomcat-runner-config.png)

6. Define a name for the configuration, we're using `Tomcat`
7. Set path to Tomcat Location
8. Add a context, we'll be using `/web-app` as path and `D:\github\dd-webapp-custom-examples\custom-webapp\target\custom-webapp-1.0-SNAPSHOT` as the Document Base.
9. Add a new Maven goal
10. Fill in `clean install` inside the command line field

![Maven goal configure](./images/maven-goal.png)

11. Close the dialog by clicking the Ok button
12. Start the custom web application by clicking on `Run` -> `Debug 'Tomcat'`

#### IntelliJ Ultimate Edition

If you have the paid version of IntelliJ there is already a Tomcat integration.

In order to debug it we'll setup a configuration for Tomcat.

1. Click on the `Run` item in the top menu
2. Open `Edit Configurations`
3. Click on the `+` icon on the top left
4. Go to `Tomcat Server` and click on `Local`

![Add Tomcat](./images/add-tomcat-config.png)

5. Define a name for the configuration, we're using `Tomcat`
6. Remove all task from the list at the bottom of the dialog
7. Add a new Maven goal

![Add maven goal](./images/add-maven-goal.png)

8. Fill in `clean install` inside the command line field

![Maven goal configure](./images/maven-goal.png)

9. Click Ok

10. Open the `deployment` tab
11. Click on the `+` icon next to the `Deploy at server startup` list

![Add war file deployment](./images/add-deployment-war.png)

12. Select the exploded war file
13. Close the dialog by clicking the Ok button

![Final configuration](./images/final-config.png)

14. Start the custom web application by clicking on `Run` -> `Debug 'Tomcat'`
