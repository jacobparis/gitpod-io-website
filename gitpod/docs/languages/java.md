<!-- NEW PAGE -->

# Getting Started / Quick Start

Gitpod comes with great support for Java.

This guide walks you through configuring a Java application with Gitpod.

<!-- TODO: @nancy -->

# Part 1: Setting up your project

## Pre-Requisites

- [Docker](https://docs.docker.com/)
- [YAML](https://yaml.org/spec/1.1/) <!-- TODO: Check if this is the correct version -->
- [Linux](https://www.linux.org/)
- [Bash](https://www.gnu.org/savannah-checkouts/gnu/bash/manual/bash.html)
- [Environment variables](https://wiki.archlinux.org/title/environment_variables)

## Getting Started

To see a full working Java application: open [gitpod-io/spring-petclinic](https://github.com/gitpod-io/spring-petclinic/) in Gitpod.

## Installing dependencies

### The Gitpod default base image: "workspace-full"

The default Gitpod workspace <!-- TODO: Link to workspace page --> image default is <!-- TODO: Link to workspace-full contents documentation --> [workspace-full](https://github.com/gitpod-io/workspace-images) based on [Ubuntu](https://ubuntu.com/).

Along with other languages and tools<!-- TODO: Link to full list -->, this base image includes:

- [SDKMAN!](https://sdkman.io/) `v5.15.0` (`sdk version`)
- [Java](https://www.java.com) `v11.0.13` (`java -version`)
- [Gradle](https://gradle.org/) `v7.4.1` (`gradle -version`)
- [Maven](https://maven.apache.org/) `v3.8.5` (`mvn -version`)

<!-- TODO: Note about OCI compliance -->

### Updating Java, Maven & Gradle versions (using SDKMAN!)

Using [SDKMAN!](https://sdkman.io/usage#listversions) you can quickly update your version dependencies:

`sdk install <candidate> [version]`

> Swapping versions manually is a quick way to explore Gitpod. However, we **strongly recommend to codify dependency versions in a gitpod.yml or Dockerfile**.

### Setting up a custom Dockerfile

To ensure Gitpod workspaces always start with the correct version,

1. Create a `.gitpod.yml`

```bash
touch .gitpod.yml
```

2. Create a custom Dockerfile

```bash
touch .gitpod.Dockerfile
```

3. Reference your newly created Dockerfile in your `.gitpod.yml`

```yaml
image:
  file: .gitpod.Dockerfile
```

4. Update your `.gitpod.Dockerfile` to install your dependency versions

```Dockerfile
FROM gitpod/workspace-full

USER gitpod

RUN bash -c ". /home/gitpod/.sdkman/bin/sdkman-init.sh && \
    sdk install java 17.0.3-ms && \
    sdk default java 17.0.3-ms"
```

5. Commit and push both `gitpod.yml` and `.gitpod.Dockerfile`

```bash
git commit -m "configuring gitpod with java" && git push
```

6. Stop and start (restart) your workspace

```bash
gp stop
```

7. Test your dependencies are correct in the new workspace

```bash
sdk current
```

**Note:** If you changes are not taking effect, ensure your workspace is building from the correct [context](/docs/context-urls), where your `gitpod.yml` or `gitpod.Dockerfile` are checked in to version control.

<!-- TODO: Validate for any more Gotchas -->

## Running the Java application

When starting a workspace, you can configure [start tasks](/docs/config-start-tasks) to be initiated on workspace start.

1. Add the command to start your application to your `.gitpod.yml`

```yaml
tasks:
  - command: java <application-entry>
```

<!-- TODO: Consider adding examples for starting with Maven/Gradle, etc-->

2. Stop and start (restart) your workspace

```bash
gp stop
```

3. Validate your commands are running

```shell
gp tasks
```

**Tip:** If you're using [VS Code Browser](/docs/ides-and-editors/vscode-browser) or [VS Code Desktop](/docs/ides-and-editors/vscode), then your tasks will open as terminal windows. You can configure their layout using the [openMode](/docs/config-start-tasks#openmode) property.

See [.gitpod.yml reference](/docs/references/gitpod-yml) for more.

<!-- TODO: Add screenshot -->

### Configuring environment variables

Gitpod supports encrypted, user-specific environment variables.

Environment variables are stored as part of your user settings and can be used to set access tokens, or pass any other kind of user-specific information to your workspaces. You can set environment variables using `gp env`, or in project and account settings.

See [environment variables](/docs/environment-variables) for more.

### Configuring ports

When your project starts a service that listens on a given port, Gitpod automatically serves traffic to this port of your application on an authenticated URL.

If you want to configure ports, such as: their visibility, what Gitpod does when it detects a new port being available, etc, you do that in the ports section of the .gitpod.yml configuration file.

For example, add the following to your `.gitpod.yml` to configure port `3000` to open in your browser on workspace start. See [ports](/docs/config-ports) for detailed information.

```yaml
ports:
  - port: 3000
    onOpen: open-browser
```

### Configuring localhost

Your development application might rely on the `localhost` hostname.

There are two main options to resolve localhost issues:

1. **Replace localhost references** - Swap `localhost` references within the application with the output of `gp url <port>`, typically via an [environment variable](/docs/environment-variables).

```yaml
tasks:
  - command: |
    export DEV_ENVIRONMENT_HOST=`gp url 3000`
    java <application-entry>
```

2. **Setup localhost port forwarding** - Connect the localhost hostname on your machine with your running workspace means you don't need to replace localhost references. This is useful if you're working with a framework that needs localhost and it cannot be reconfigured.

   - If you setup [local companion](/docs/ides-and-editors/local-companion) for your workspace all ports will be forwarded automatically.
   - With [VS Code Desktop](/docs/ides-and-editors/vscode), remote port-forwarding is handled automatically and can be configured via the ports view within VS Code Desktop.
   - With JetBrains IDEs using [JetBrains Gateway](/docs/ides-and-editors/jetbrains-gateway) you can setup remote port-forwarding manually.

For detailed information on remote port-forwarding, please see [configure ports](/docs/config-ports).

### Configuring JetBrains plugins

To set default plugins to be installed for all users starting a workspace for the project, add a list of the JetBrains plugin identifiers to your `.gitpod.yml` under `vscode.extensions`.

<!-- TODO: Add list of popular Java plugins -->
<!-- TODO: Update with note about how plugin settings sync works with JetBrains -->

```yaml
vscode:
  extensions:
    - redhat.java
    - vscjava.vscode-java-debug
    - vscjava.vscode-java-test
    - pivotal.vscode-spring-boot
```

See [.gitpod.yml reference](/docs/references/gitpod-yml) for more.

### Configuring VS Code plugins

To set default extensions to be installed for all users starting a workspace for the project, add a list of the VS Code extension identifiers to your `.gitpod.yml`.

<!-- TODO: Add list of popular VS Code plugins -->

```yaml
jetbrains:
  intellij:
    plugins:
      - com.intellij.lang.jsgraphql
```

**Tip:** To ensure changes made to settings on [VS Code Desktop](/docs/ides-and-editors/vscode) are synced with [VS Code Browser](/docs/ides-and-editors/vscode-browser), make sure to enable [VS Code Settings Sync](/docs/ides-and-editors/settings-sync).

See [.gitpod.yml reference](/docs/references/gitpod-yml) for more.

<!-- ARCHIVE / OLD CONTENT -->

---

https://sdkman.io/---
section: languages
title: Java in Gitpod

---

<script context="module">
  export const prerender = true;
</script>

# Java in Gitpod

Gitpod comes with great support for Java builtin. Still, depending on your particular project you might want to further optimize the experience.

A great example of a ready-to-code Java dev environment is our [Spring Petclinic Example](https://github.com/gitpod-io/spring-petclinic).

## Ready-to-code Dev Environments for Java

To set up your project, you should run `gp init` in the terminal first. This will create the two configuration files needed to explain Gitpod what your project needs.
A dev environment is based on a Docker image. But don't fear you don't need to understand Docker to get started. In most cases, the default Docker image is enough.

### SDKMAN

For Java, it comes readily equipped with [SDKMAN!](https://sdkman.io/), which is a version manager that takes care of managing and installing not only different Java versions but also a different version of other JVM languages such as Scala or Kotlin (both supported in Gitpod, too).

It also installs Maven and Gradle in case you don't use the [wrapper version](https://docs.gradle.org/current/userguide/gradle_wrapper.html) that is often recommended.

Check out the [documentation of SDKMAN!](https://sdkman.io/usage) to see how to use it or simply type `sdk help` in the terminal.

#### SDKMAN in Docker

Although you can use sdk in your terminal, you should put the tools and versions into your Dockerfile, so you and your team get the very same configuration every time.
To do that you need to alter the generated `.gitpod.Dockerfile`. Here's an example that will install Java 12 with the J9 VM from "Adopt a JDK".

```dockerfile
FROM gitpod/workspace-full

RUN bash -c ". /home/gitpod/.sdkman/bin/sdkman-init.sh \
             && sdk install java 12.0.1.j9-adpt"
```

> Note that you always need to run the `sdkman-init.sh` in bash before you can use SDK.

You can add additional tools and versions like this:

```dockerfile
FROM gitpod/workspace-full

RUN bash -c ". /home/gitpod/.sdkman/bin/sdkman-init.sh \
             && sdk install java 12.0.1.j9-adpt \
             && sdk ..."
```

### Prebuilds

Gitpod provides disposable dev environments, which means you are getting fresh developer environments for every task. So configuring them to be ready-to-code is crucial to get the most out of Gitpod.

In the generated `.gitpod.yml` you will find the following section:

```yml
# List the startup tasks. You can start them in parallel in multiple terminals. See https://www.gitpod.io/docs/config-start-tasks/
tasks:
  - init: echo 'init script' # runs during prebuild
    command: echo 'start script'
```

You can have as many tasks as you which, each will result in an opened terminal when you start a dev environment.

Each task supports multiple phases, most importantly `before`, `init`, `command`. To be ready to code a dev environment should not only be in sync with the Git repo but also needs to have the currently checked out state built.
For instance, in the [Spring Petclinic demo](https://github.com/gitpod-io/spring-petclinic) the command section looks like this:

```yml
# startup tasks
tasks:
  - init: ./mvnw package -DskipTests
    command: java -jar target/*.jar
```

Since we have installed the [Gitpod app](https://github.com/apps/gitpod-io) on that GitHub repository, Gitpod will prebuild any branch as soon as it starts. During a prebuild it will

1.  start a container based on the Docker image,
2.  clone the repository and check out the respective branch,
3.  run the `before` and `init` parts of every task,
4.  capture the result and store it

Once you or your teammates start a dev environment, you will get the prebuild state. The log output from `init` is still presented in the terminal but it will have two additional lines, e.g.:

```sh
🍌 This task ran as part of a workspace prebuild.
🎉 You just saved 4 minutes of watching your code build.
```

## IDE features

### Debugging

Here is a quick clip on how to automatically configure debugging for Java!

![Java debugging example](../../../static/images/docs/JavaDebug.gif)

So, basically in this video we:

1. First, open the Java file that we want to debug
2. Then, go to the debug menu and select "Add Configuration..."
3. Next, in the dropdown choose "Java: Launch Program in Current File"
4. Finally, start debugging your Java program!

You can also create the Java debug configuration file manually

To start debugging your Java application in Gitpod, please create a new directory called `.theia/`, and inside add a file called `launch.json`, finally add the following to it:

```json
{
  // Use IntelliSense to learn about possible attributes.
  // Hover to view descriptions of existing attributes.
  "version": "0.2.0",
  "configurations": [
    {
      "type": "java",
      "name": "Debug (Launch) - Current File",
      "request": "launch",
      "mainClass": "${file}"
    }
  ]
}
```

Then, simply open the Java file you want to debug, open the Debug panel (in the left vertical toolbar, click the icon with the crossed-out-spider), and click the green "Run" button.

<br>

To see a basic repository with Java debugging enabled, please check out [gitpod-io/Gitpod-Java-Debugging](https://github.com/gitpod-io/Gitpod-Java-Debugging):

[![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#https://github.com/gitpod-io/Gitpod-Java-Debugging)

For more please see [VSCode's docs](https://code.visualstudio.com/docs/java/java-debugging)

### VSCode extensions

Gitpod comes equipped with the following VS Code extensions:

- [Language Support for Java(TM)](https://marketplace.visualstudio.com/items?itemName=redhat.java)
- [Debugger for Java](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-debug)
- [Java Dependency Viewer](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-dependency)

You can [install additional extensions](/docs/ides-and-editors/vscode-extensions) for your project if you want.

Most of the information you find in the [Java for VS Code](https://code.visualstudio.com/docs/languages/java) documentation applies to Gitpod as well.

## Vaadin in Gitpod

To work with Vaadin in Gitpod, you will need to properly configure your repository. Here is how to do it.

### Example Repositories

Here are a few Vaadin example projects that are already automated with Gitpod:

<div class="overflow-x-auto">

| Repository                                                               | Description                               | Try it                                                                                                                                      |
| ------------------------------------------------------------------------ | ----------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| [Vaadin CRM](https://github.com/vaadin-learning-center/crm-tutorial)     | Full-stack Vaadin and Spring Boot         | [![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#https://github.com/vaadin-learning-center/crm-tutorial) |
| [Vaadin starter](https://github.com/vaadin/skeleton-starter-flow-spring) | An empty starter for a new Vaadin project | [![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#https://github.com/vaadin/skeleton-starter-flow-spring) |

</div>

### Configuring Gitpod for a Vaadin project

Start by downloading a [**Vaadin and Spring Boot** project starter](https://vaadin.com/start) if you don't have a Vaadin project from before.

Next, add a [`.gitpod.yml`](/docs/config-gitpod-file) file with the follwing content to the root of the project:

```YAML
tasks:
  - command: mvn spring-boot:run
ports:
  - port: 8080
    onOpen: open-preview
```

This will start the development server and open up the application in the preview browser window.

You are now ready to push your code to GitHub.

### Enable prebuilds for a faster startup

The first time you start a Vaadin application, it downloads both Maven and npm dependencies, which can take some time. You can speed up the GitPod startup by enabling [prebuild](/docs/prebuilds) for the repository.

Update your [`.gitpod.yml`](/docs/config-gitpod-file) file with the following content:

```YAML
tasks:
  - prebuild: mvn install spring-boot:start spring-boot:stop -DskipTests
    command: mvn spring-boot:run
ports:
  - port: 8080
    onOpen: open-preview
github:
  prebuilds:
    master: true
```

Finally, enable prebuilds on GitHub [as instructed in the prebuild documentation](/docs/prebuilds#enable-prebuilt-workspaces).

Next time you push code to your repository, the `prebuild` command will download all the dependencies and make the project startup faster.
