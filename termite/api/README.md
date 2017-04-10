# com-mttrbit-termite::plugin::api
This plugin is teh blueprint for the standardized build workflow.
Further plugins are going to override the activities defined within this plugin.

When you set the log level to debug, this plugin prints a few messages indicating
which activities are nt overwritten.

## API

### clean
delete any artifacts from previous builds

#### clean:pre
execute processes needed prior to the actual project cleaning

#### clean:clean
delete any artifacts from previous builds

#### clean:post
execute processes needed to finalize the project cleaning

### validate
validate the project is correct and all necessary information is available

### initialize
initialize build state, e.g. set properties or create directories

### compile
compile the source code of the project

#### compile:generate-resources
generate resources for inclusion in the package

#### compile:process-resources
copy and process the resources into the destination directory, ready for packaging

#### compile:generate-sources
generate any source code for inclusion in compilation

#### compile:process-sources
process the source code, for example to filter any values

#### compile:compile
compile the source code of the project

#### compile:process-classes
post-process the generated files from compilation, for example to do bytecode enhancement on Java classes

#### compile:provision
supply provision required by this project

### test
compile the source code of the project

#### test:generate-resources
generate any resources for inclusion in test compilation

#### test:process-resources
copy and process the resources into the test destination directory

#### test:generate-sources
generate any source code for inclusion in test compilation

#### test:process-sources
process the test source code, for example to filter any values

#### test:compile
compile the test source code into the test destination directory

#### test:provision
supply provision required to test this project

#### test:test
run tests using a suitable unit testing framework. These tests should not require the code be packaged or deployed

### package
take the compiled code and package it in its distributable format, such as a JAR

#### package:prepare
perform any operations necessary to prepare a package before the actual packaging. This often results in an unpacked, processed version of the package

#### package:package
take the compiled code and package it in its distributable format, such as a JAR

### it
run integration tests

#### it:pre
perform actions required before integration tests are executed. This may involve things such as setting up the required environment

#### it:it
process and deploy the package if necessary into an environment where integration tests can be run

#### it:post
perform actions required after integration tests have been executed. This may including cleaning up the environment

### site
generate documentation

#### site:pre
execute processes needed prior to the actual project site generation

#### site:site
generate documentation

#### site:post
execute processes needed to finalize the site generation, and to prepare for site deployment

#### site:deploy
deploy the generated site documentation to the specified web server

### verify
run any checks to verify the package is valid and meets quality criteria

### publish:local
publish the package into the local repository

#### publish:local:prepare
prepare a publish for a local repository

#### publish:local:generate-version
generate a local version number

#### publish:local:publish
publish the package into the local repository, for use as a dependency in other projects locally

### publish:shared
publish for a shared repository (snapshot)

#### publish:shared:prepare
prepare a publish for a shared repository (snapshot)

#### publish:shared:generate-version
generate a version number for shared publication

#### publish:shared:publish
done in an integration environment, copies the final package to the remote repository for sharing with other developers and projects

### release
copies the final package to the remote repository

#### release:prepare
prepare a release

#### release:generate-version
generate a version number for a release

#### release:release
done in an integration or release environment, copies the final package to the remote repository for sharing with other developers and projects

### report
generate report