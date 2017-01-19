The setup described here is useful if you want to create a custom project that extends OpenRemote, and keep that extensions separate in a possibly private Git repository.

Your service provider code and project-specific configuration can be managed independently. You can track and merge changes on the upstream main OpenRemote project, targeting specific tags for your integration or always keeping up with the main development branch.

## Preparing the project repositories

You need a checkout of the main [OpenRemote project](https://github.com/openremote/openremote.git).

Then, create a new git project directory or remote repository. After changing into the checked out main OpenRemote project directory, add your project repository as a [submodule](https://git-scm.com/book/en/v2/Git-Tools-Submodules):

```
git clone https://github.com/openremote/openremote.git
cd openremote/
git submodule add <URL or relative local path of your project repository>
```

The generated `.gitmodules` file contains a list of all submodules in your main project.

In IntelliJ IDEA open `Settings` > `Version Control` and add the new submodule directory as a Git repository. IntelliJ will now pull and push automatically on the corresponding remote repository when any changes are made on submodules.

## Extending the build

Adding your project's repository as a submodule is the first step for integration. You should also include it in the regular Gradle build, so you can depend directly on the code in the main project. That way any changes you make to the main code of OpenRemote will be available immediately in your IDE in your project code that depends on it.

For example, in your project use the following dependencies to implement an OpenRemote agent:

```
dependencies {

    // If this is a git submodule use a project dependency, otherwise use artifact
    compile project.parent != null ? project(":agent") : "org.openremote:openremote-agent:3.0-SNAPSHOT"
}
```

When you work with Gradle in your custom project directory, you'll depend on the published and versioned OpenRemote agent SPI artifact. However, if you work with Gradle in the root, main OpenRemote project, your custom project will use an internal project dependency on the agent SPI code.

