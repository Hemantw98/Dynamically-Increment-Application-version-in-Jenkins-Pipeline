## Create a CI Pipeline with Jenkinsfile (Freestyle, Pipeline, Multibranch Pipeline)

### Technologies used:
Jenkins, Docker, Linux, Git, Java, Maven

### Project Description:

CI Pipeline for a Java Maven application to build and push to the repository

  Install Build Tools (Maven, Node) in Jenkins
  Make Docker available on Jenkins server
  Create Jenkins credentials for a git repository
  
  Create different Jenkins job types (Freestyle, Pipeline, Multibranch pipeline) for the Java Maven project with Jenkinsfile to:
  
 1. Connect to the application’s git repository 
 2. Build Jar
 3. Build Docker Image
 4. Push to private DockerHub repository
 
 
 Here's a detailed explanation of how to create a CI pipeline with Jenkinsfile for different Jenkins job types, including Freestyle, Pipeline, and Multibranch Pipeline:
 
 ### Freestyle Job:
 
 A Freestyle job in Jenkins allows you to create a custom build pipeline with sequential build steps. Here are the steps to create a Freestyle job with Jenkinsfile:
 
Step 1: Create a new Freestyle job in Jenkins:

Click on "New Item" on the Jenkins home page and give your job a name.
Select "Freestyle project" and click on "OK" to create the job.
Step 2: Configure the job:

In the job configuration page, under the "Source Code Management" section, select the appropriate SCM system (e.g., Git) and provide the repository URL and credentials to access your Java Maven application's git repository.
Under the "Build" section, add build steps to build your Maven application, such as running Maven commands to build the JAR file. For example:

    mvn clean install

Optionally, you can add build steps to build and push Docker image, and push the JAR and Docker image to the repository as needed. For example:

        docker build -t my-image .
    docker push my-image:tag
    
Save the job configuration.

### Pipeline Job:

A Pipeline job in Jenkins allows you to define your entire build pipeline as a script using the Jenkinsfile. Here are the steps to create a Pipeline job with Jenkinsfile:

Step 1: Create a new Pipeline job in Jenkins:

Click on "New Item" on the Jenkins home page and give your job a name.
Select "Pipeline" and click on "OK" to create the job.
Step 2: Define the pipeline script in Jenkinsfile:

In the job configuration page, under the "Pipeline" section, select "Pipeline script" and enter the pipeline script in the text area.
The Jenkinsfile can include stages for building the JAR, building the Docker image, and pushing the JAR and Docker image to the repository, using the credentials created earlier. Here's an example Jenkinsfile:

    pipeline {
        agent any

        stages {
            stage('Build') {
                steps {
                    sh 'mvn clean install'
                }
            }
            stage('Build Docker Image') {
                steps {
                    sh 'docker build -t my-image .'
                }
            }
            stage('Push Docker Image') {
                steps {
                    withCredentials([usernamePassword(credentialsId: 'your-git-credentials', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                        sh 'docker login -u $GIT_USERNAME -p $GIT_PASSWORD'
                        sh 'docker push my-image:tag'
                    }
                }
            }
        }
    }

Save the job configuration.

### Multibranch Pipeline Job:

A Multibranch Pipeline job in Jenkins allows you to automatically create pipelines for each branch in your git repository. Here are the steps to create a Multibranch Pipeline job with Jenkinsfile:
Step 1: Create a new Multibranch Pipeline job in Jenkins:

Click on "New Item" on the Jenkins home page and give your job a name.
Select "Multibranch Pipeline" and click on "OK" to create the job.

Step 2: Configure the job:

In the job configuration page, under the "Branch Sources" section, specify the SCM system (e.g., Git) and provide the repository URL and credentials to access your Java Maven application's git repository.
Under the "Build Configuration" section, select "by Jenkinsfile" and provide the path to your Jenkinsfile in the "Script Path" field.

### Configure Webhooks:

Configure a webhook to trigger the pipeline job automatically whenever a new code is pushed to the Git repository.

### Test and Verify:

Once you have configured the pipeline job, test and verify it by making a change in the Git repository and pushing it to the repository. Check if the pipeline job is triggered and executes successfully.

Here are the steps for creating a CI Pipeline for a Java Maven application to build and push to the repository:

Install Build Tools (Maven, Node) in Jenkins:

Log in to your Jenkins server and navigate to Manage Jenkins > Global Tool Configuration.
Scroll down to the "Maven" section and click "Add Maven".
Enter a name for the Maven installation and select the version you want to install.
Repeat the process for Node.js and npm if needed.
Click "Save" to save the configuration.
Make Docker available on Jenkins server:

Install Docker on your Jenkins server if it is not already installed.
Add the Jenkins user to the "docker" group:

    sudo usermod -aG docker jenkins
    sudo systemctl restart jenkins

Create Jenkins credentials for a Git repository:

Navigate to Jenkins > Credentials > System > Global credentials (unrestricted).
Click "Add Credentials".
Select the appropriate credential type (e.g. "Username with password" for a Git username and password).
Enter the necessary information and click "OK".
Create different Jenkins job types (Freestyle, Pipeline, Multibranch pipeline) for the Java Maven project with Jenkinsfile to:
a. Connect to the application's Git repository:

Create a new Jenkins job (e.g. "Java-Maven-Freestyle").

Select "Freestyle project" as the job type.

Enter a name for the job and click "OK".

Under "Source Code Management", select "Git".

Enter the repository URL and select the appropriate Git credentials.

Optionally, specify a branch or tag to build.

b. Build Jar:

Under "Build", click "Add build step" and select "Invoke top-level Maven targets".

In the "Goals" field, enter "clean package".

Click "Save" to save the job configuration.

c. Build Docker Image:

Install the "Docker Build and Publish" plugin if it is not already installed.

Under "Build", click "Add build step" and select "Docker Build and Publish".

Enter a name for the Docker image and specify the Dockerfile location.

Optionally, specify any build arguments or Docker registry credentials.

Click "Save" to save the job configuration.

d. Push to private DockerHub repository:

Under "Post-build Actions", click "Add post-build action" and select "Push Docker Image".

Specify the Docker registry credentials and the Docker image name and tag.

Click "Save" to save the job configuration.

To create a Pipeline or Multibranch Pipeline job:

a. Create a new Jenkins job.

b. Select "Pipeline" or "Multibranch Pipeline" as the job type.

c. Enter a name for the job and click "OK".

d. In the job configuration, specify the Jenkinsfile location and any necessary parameters or variables.

e. Save the job configuration.

That's it! You now have a CI Pipeline set up for your Java Maven application that builds and pushes Docker images to a private registry.
