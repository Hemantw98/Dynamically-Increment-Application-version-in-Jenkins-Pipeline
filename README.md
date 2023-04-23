## Dynamically Increment Application version in Jenkins Pipeline

### Technologies used:

Jenkins, Docker, GitLab, Git, Java, Maven

### Project Description:

Configure CI step: Increment patch version

Configure CI step: Build Java application and clean old artifacts

Configure CI step: Build Image with dynamic Docker Image Tag

Configure CI step: Push Image to private DockerHub repository

Configure CI step: Commit version update of Jenkins back to Git repository

Configure Jenkins pipeline to not trigger automatically on CI build commit to avoid commit loop

Let's break down the steps for dynamically incrementing the application version in a Jenkins pipeline. We'll assume that you have a Jenkins server set up and configured with plugins for Docker, GitLab, and Maven.

### Step 1: Configure CI step - Increment patch version

This step involves incrementing the patch version of your Java application. You can use a plugin like "Version Number Plugin" in Jenkins to achieve this. Here are the commands to increment the patch version using Maven:

    mvn build-helper:parse-version versions:set \
    -DnewVersion=\${parsedVersion.majorVersion}.\${parsedVersion.minorVersion}.\${parsedVersion.nextIncrementalVersion} \
    versions:commit
    
This command uses Maven's build-helper and versions plugins to parse the current version of the application from the pom.xml file, increment the patch version, and commit the changes back to the pom.xml file.

### Step 2: Configure CI step - Build Java application and clean old artifacts

This step involves building your Java application and cleaning old artifacts. You can use the following commands:

    mvn clean install
    
This command uses Maven to clean the project and build the Java application, generating artifacts like JAR files.

### Step 3: Configure CI step - Build Image with dynamic Docker Image Tag

This step involves building a Docker image with a dynamic Docker image tag that includes the application version. You can use the following commands: 

    docker build -t myapp:${parsedVersion.majorVersion}.${parsedVersion.minorVersion}.${parsedVersion.incrementalVersion} .

This command uses Docker to build an image from the current directory (.) with a tag that includes the major, minor, and incremental version numbers parsed from the pom.xml file.

### Step 4: Configure CI step - Push Image to private DockerHub repository

This step involves pushing the Docker image to a private DockerHub repository. You can use the following commands:

    docker login -u <username> -p <password>
    docker push myapp:${parsedVersion.majorVersion}.${parsedVersion.minorVersion}.${parsedVersion.incrementalVersion}
    
These commands first login to DockerHub with the provided username and password, and then push the Docker image to the private repository with the dynamic image tag.

### Step 5: Configure CI step - Commit version update of Jenkins back to Git repository
This step involves committing the updated version number back to the Git repository. You can use the following commands:

    git add pom.xml
    git commit -m "Increment version number to ${parsedVersion.majorVersion}.${parsedVersion.minorVersion}.${parsedVersion.incrementalVersion}"
    git push origin <branch-name>
    
These commands add and commit the changes made to the pom.xml file, including the updated version number, and push the changes back to the Git repository on the specified branch.

### Step 6: Configure Jenkins pipeline to not trigger automatically on CI build commit
To avoid a commit loop, you can configure your Jenkins pipeline not to trigger automatically on a CI build commit. You can use the following command in your Jenkinsfile:

    properties([
      pipelineTriggers([]) // Disable automatic triggering on commit
    ])

This command disables the automatic triggering of the Jenkins pipeline on a CI build commit, preventing a loop of continuous builds triggered by each commit.

That's it! You've successfully demonstrated the complete steps for dynamically incrementing the application version in a Jenkins pipeline using Jenkins, Docker, GitLab, Git, Java, and Maven. Please note that this is just a high-level overview, and you may need to adapt the commands to fit your specific setup and requirements.
   

    




