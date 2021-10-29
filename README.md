# GitHub Advanced Security Info Dump
## A repository of information, links, scripts, and advice pertaining to GitHub Advanced Security, aka GHAS

## Code Scanning

### Intro
Code Scanning is freely available for use in public repositories.  You should try it out.  Learn about application security and how to write secure code by cloning a free, vulnerable repository.  Then scan it using GitHub Advanced Security and fix the problems.  

Here are some good vulnerable open source repositories you can clone to give code scanning a try:
 - [NodeGoat](https://github.com/OWASP/NodeGoat)
 - [WebGoat](https://github.com/WebGoat/WebGoat)
 - [DotNetGoat](https://github.com/jerryhoff/WebGoat.NET)
 - [Pygoat](https://github.com/adeyosemanputra/pygoat)
 - [JuiceShop](https://github.com/juice-shop/juice-shop.git)

If you're an open source project maintainer on GitHub, you should definitely enable it if your project is using a supported language.  You may already be using a free static analyzer (SAST) - great.  If it outputs to SARIF, you can use both and integrate it with the Security Dashboard.  I think you'll find the results from GHAS Code Scanning to be compelling.  

### Code Scanning Setup Guide
It's really easy to set up code scanning on a repository.  Just go to the security tab of your repository, scroll down to the code scanning section and select "Set up Code Scanning".  You will then be brought to a curated list of static analyzers that can publish results to the Security Dashboard.  At the top is CodeQL.  Select that one to create a codeql-analysis.yml file.  Most of the time, you can just accept the defaults. However, if you do need to change some of the settings, you can view this documentation:  https://github.com/github/codeql-action

Commit this file to main/master or create a pull request and merge it.  Once you do that, code scanning will start publishing results to the security dashboard.  

Here's a quick video from Coder Dave showing this exact process: https://www.youtube.com/watch?v=A8SERCUE-i4

### Some Quick Troubleshooting
**If you do not get results:**
You may need to change the query set.  The default query set is called ```security```.  There's also ```security-extended``` and ```security-and-quality```.  ```security-extended``` looks for more, lower-severity security vulnerabilities.  Just change the "Intialize CodeQL" block to read:
```
 - name: Initialize CodeQL
      uses: github/codeql-action/init@v1
      with:
        queries: +security-extended
```

**If your codeql analysis action fails to complete:**
You may need to specify a build command or build process.  Refer to the documentation linked above.  Here is an example of how you might specify a build command.  Comment out the autobuild specification in the yml file and specify the build commands one would use to fully re-compile the application.
```
#   This repo fails to use the Autobuild from CodeQL
#    - name: Autobuild
#      uses: Anthophila/codeql-action/codeql/autobuild@master
    
    - name: Set up JDK 1.11
      uses: actions/setup-java@v1
      with:
        java-version: 1.11
    
    - name: Build with Maven
      run: mvn -B package --file pom.xml
 ```


