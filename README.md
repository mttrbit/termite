# mttrbit ant scripts
Defines a standardized build workflow in terms of ant targets. These targets are enriched by plugins.

## Features
- Standardized Build workflow
- Extensible workflow
- Fully automated

## Install
Your project contains both an ivy.xml file and a module.ant files similar to the ones listed below. Within the directory
of the project you execute on cli:
```
$ ant -f module.ant
```
This will initialize the pdsm ant build workflow.

Afterwards you can use the targets as stated in teh section api. For example to compile your code run:
```
$ ant -f module.ant compile
```

## Usage
You need two files to start working:
- ivy.ml - your module definition
- module.ant - build definition

### ivy.xml
You set project specific values such as organisation, module, version, and status via doctype. You define dependencies without relying on external property files.
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE ivy-module [
        <!ENTITY organisation "com.mttrbit">
        <!ENTITY module "mttrbit-termite-example">
        <!ENTITY version "1.0.0-SNAPSHOT">
        <!ENTITY status "integration">
        ]>
<ivy-module version="2.0"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:noNamespaceSchemaLocation="http://ant.apache.org/ivy/schemas/ivy.xsd"
            xmlns:m="http://ant.apache.org/ivy/maven">

    <info organisation="&organisation;" module="&module;" revision="&version;" status="&status;">
        <description/>
        <m:maven.plugins>
            org.codehaus.mojo__dependency-Maven-plugin__null|org.apache.Maven.plugins__Maven-clean-plugin__null|org.apache.Maven.plugins__Maven-jar-plugin__null|org.apache.Maven.plugins__Maven-source-plugin__null
        </m:maven.plugins>
    </info>
    <configurations>
        <conf name="default" visibility="public"
              description="runtime dependencies and master artifact can be used with this conf"
              extends="runtime,master"/>
        <conf name="master" visibility="public"
              description="contains only the artifact published by this module itself, with no transitive dependencies"/>
        <conf name="compile" visibility="public"
              description="this is the default scope, used if none is specified. Compile dependencies are available in all classpaths."/>
        <conf name="provided" visibility="public"
              description="this is much like compile, but indicates you expect the JDK or a container to provide it. It is only available on the compilation classpath, and is not transitive."/>
        <conf name="runtime" visibility="public"
              description="this scope indicates that the dependency is not required for compilation, but is for execution. It is in the runtime and test classpaths, but not the compile classpath."
              extends="compile"/>
        <conf name="test" visibility="private"
              description="this scope indicates that the dependency is not required for normal use of the application, and is only available for the test compilation and execution phases."
              extends="runtime"/>
        <conf name="system" visibility="public"
              description="this scope is similar to provided except that you have to provide the JAR which contains it explicitly. The artifact is always available and is not looked up in a repository."/>
        <conf name="sources" visibility="public"
              description="this configuration contains the source artifact of this module, if any."/>
        <conf name="javadoc" visibility="public"
              description="this configuration contains the javadoc artifact of this module, if any."/>
        <conf name="optional" visibility="public" description="contains all optional dependencies"/>
    </configurations>

    <publications defaultconf="runtime">
        <artifact name="&module;" ext="jar" conf="master"/>
        <artifact name="&module;" type="pom" ext="pom" />
        <artifact name="&module;" ext="jar" type="sources" conf="sources" m:classifier="sources"/>
    </publications>

    <dependencies>
        <dependency org="org.testng" name="testng" rev="6.9.10" force="true" conf="test->default(*)"/>
        <dependency org="org.hamcrest" name="hamcrest-all" rev="1.3" conf="test->default(*)"/>
        ...
    </dependencies>
</ivy-module>
```

### module.ant
This file defines the starting point for the build workflow.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project name="mttrbit-termite-example" default="termite:init" basedir="." xmlns:unless="ant:unless" xmlns:if="ant:if">
    <import>
        <javaresource name="api.xml">
            <classpath location="${user.home}/.termite/termite.jar"/>
        </javaresource>
    </import>

    <!-- loads three plugins: java with testng -->
    <load-plugin name="java" if:set="dir.exists"/>
    <load-plugin name="testng" if:set="dir.exists"/>
</project>
```

## API

### clean
delete any artifacts from previous builds

### validate
validate the project is correct and all necessary information is available


### compile
compile the source code of the project

### test
compile the source code of the project

### package
take the compiled code and package it in its distributable format, such as a JAR

### it
run integration tests

### site
generate documentation

#### site:deploy
deploy the generated site documentation to the specified web server

### verify
run any checks to verify the package is valid and meets quality criteria

### publish:local
publish the package into the local repository

### publish:shared
publish for a shared repository (snapshot)

### release
copies the final package to the remote repository

### report
generate report