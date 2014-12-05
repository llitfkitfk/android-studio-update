Gradle Plugin User Guide
=====================

1. [Introduction](#Introduction)
	1. Goals of the new Build System 
	2. Why Gradle?
2. Requirements
3. Basic Project
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

4. Dependencies, Android Libraries and Multi-project setup
	1. Dependencies on binary packages
		1. Local packages
		2. Remote artifacts
	2. Multi project setup
	3. Library projects
		1. Creating a Library Project
		2. Differences between a Project and a Library Project
		3. Referencing a Library
		4. Library Publication
5. Testing
	1. Basics and Configuration
	2. Running tests
	3. Testing Android Libraries
	4. Test reports
		1. Single projects
		2. Multi-projects reports
	5. Lint support
6. Build Variants
	1. Product flavors
	2. Build Type + Product Flavor = Build Variant
	3. Product Flavor Configuration
	4. Sourcesets and Dependencies
	5. Building and Tasks
	6. Testing
	7. Multi-flavor variants
7. Advanced Build Customization
	1. Build options
		1. Java Compilation options
		2. aapt options
		3. dex options
	2. Manipulating tasks
	3. BuildType and Product Flavor property reference
	4. Using sourceCompatibility 1.7



#Introduction

This documentation is for the Gradle plugin version 0.9. Earlier versions may differ due to non-compatible we are introducing before 1.0.
