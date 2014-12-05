Gradle Plugin User Guide
=====================

1. [Introduction](#introduction)
	1. Goals of the new Build System 
	2. Why Gradle?
2. [Requirements](#requirements)
3. [Basic Project](#basic-project)
	1. Simple build files
	2. Project Structure
		1. Configuring the Structure 
	3. Build Tasks
		1. General Tasks 
		2. Java project tasks
		3. Android tasks
	4. Basic Build Customization
		1. Manifest entries
		2. Build Types
		3. Signing Configurations
		4. Running ProGuard

4. [Dependencies, Android Libraries and Multi-project setup](#dependencies-android-libraries-and-multi-project-setup)
	1. Dependencies on binary packages
		1. Local packages
		2. Remote artifacts
	2. Multi project setup
	3. Library projects
		1. Creating a Library Project
		2. Differences between a Project and a Library Project
		3. Referencing a Library
		4. Library Publication
5. [Testing](#testing)
	1. Basics and Configuration
	2. Running tests
	3. Testing Android Libraries
	4. Test reports
		1. Single projects
		2. Multi-projects reports
	5. Lint support
6. [Build Variants](#build-variants)
	1. Product flavors
	2. Build Type + Product Flavor = Build Variant
	3. Product Flavor Configuration
	4. Sourcesets and Dependencies
	5. Building and Tasks
	6. Testing
	7. Multi-flavor variants
7. [Advanced Build Customization](#advanced-build-customization)
	1. Build options
		1. Java Compilation options
		2. aapt options
		3. dex options
	2. Manipulating tasks
	3. BuildType and Product Flavor property reference
	4. Using sourceCompatibility 1.7



#Introduction [\^](#gradle-plugin-user-guide)

This documentation is for the Gradle plugin version 0.9. Earlier versions may differ due to non-compatible we are introducing before 1.0.

```
这个文档的gradle 插件版本是0.9
在1.0版本之前的早期版本因为不兼容可能会有些不同
```

##Goals of the new Build System

The goals of the new build system are:
Make it easy to reuse code and resources
Make it easy to create several variants of an application, either for multi-apk distribution or for different flavors of an application
Make it easy to configure, extend and customize the build process
Good IDE integration

##Why Gradle?

Gradle is an advanced build system as well as an advanced build toolkit allowing to create custom build logic through plugins.

Here are some of its features that made us choose Gradle:
Domain Specific Language (DSL) to describe and manipulate the build logic
Build files are Groovy based and allow mixing of declarative elements through the DSL and using code to manipulate the DSL elements to provide custom logic.
Built-in dependency management through Maven and/or Ivy.
Very flexible. Allows using best practices but doesn’t force its own way of doing things.
Plugins can expose their own DSL and their own API for build files to use.
Good Tooling API allowing IDE integration


#Requirements [\^](#gradle-plugin-user-guide)

Gradle 1.10 or 1.11 or 1.12 with the plugin 0.11.1
SDK with Build Tools 19.0.0. Some features may require a more recent version.


#Basic Project [\^](#gradle-plugin-user-guide)

#Dependencies, Android Libraries and Multi-project setup [\^](#gradle-plugin-user-guide)

#Testing [\^](#gradle-plugin-user-guide)

#Build Variants [\^](#gradle-plugin-user-guide)

#Advanced Build Customization [\^](#gradle-plugin-user-guide)