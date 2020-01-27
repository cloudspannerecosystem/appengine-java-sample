# App Engine Java 11 sample
[![cloudspannerecosystem](https://circleci.com/gh/cloudspannerecosystem/appengine-java-sample.svg?style=svg)](https://circleci.com/gh/cloudspannerecosystem/appengine-java-sample)

This sample demonstrates how to use Cloud Spanner from the
[App Engine Standard with Java 11 environment](https://cloud.google.com/appengine/docs/java/).

## Prerequisites

### Create a project in the Google Cloud Platform Console

If you haven't already created a project, create one now.

1. Open the [Cloud Console](https://console.cloud.google.com/).
1. In the drop-down menu at the top, select **Create a project**.
1. Give your project a name.
1. Make a note of the project ID, which might be different from the project
   name. The project ID is used in commands below.

If you like, you can activate a
[Cloud Shell](https://cloud.google.com/shell/docs/) for your project now.
When using Cloud Shell, the Google Cloud SDK and Maven in the steps below will
already be installed.

### Set up your Google Cloud Platform Project

Install the [Google Cloud SDK](https://cloud.google.com/sdk/) if it's not
already available in your enviromment. Then run:

    gcloud init

If this is your first time creating an App engine application:

    gcloud app create

### Install Maven

This sample uses the [Apache Maven](https://maven.apache.org) build system.
If Maven isn't installed in your environment yet,
[download](https://maven.apache.org/download.cgi) and
[install](https://maven.apache.org/install.html) it.
When you use Maven as described here, it will automatically download the
required client libraries.

### Google Cloud Shell Open JDK 11 setup:

To switch to Open JDK 11 in a Cloud Shell session, run:

    # Select the usr/lib/jvm/java-11-openjdk-amd64/bin/java version.
    sudo update-alternatives --config java
    # Set the JAVA_HOME variable for Maven to pick the correct JDK:
    export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64

## Set up the sample

Clone the repository:

    git clone https://github.com/cloudspannerecosystem/appengine-java-sample

Compile the [simple-jetty-main](simple-jetty-main/README.md) `Main` class:

    cd appengine-java-sample/simple-jetty-main
    mvn install

If this fails with an error about the Java target version, make sure you've
switched to the Open JDK 11 as described above.

Change to the `appengine-java-sample` directory:

    cd ..

Next, [create a Spanner instance](https://cloud.google.com/spanner/docs/quickstart-console#create_an_instance)
and update the `<SPANNER_INSTANCE>` value in
[`app.yaml`](src/main/appengine/app.yaml) with your instance ID.

## Deploying

Replace `<PROJECT_ID>` below with your project ID:

    mvn package appengine:deploy -Dapp.deploy.projectId=<PROJECT_ID>

## Endpoints

`/spanner`: will run a number of sample operations against the Spanner instance.
Individual tasks can be run using the `task` query parameter.
See [SpannerTasks](src/main/java/com/example/appengine/spanner/SpannerTasks.java)
for the supported set of tasks.

To execute the endpoint handler of the deployed sample application, open
`https://<PROJECT_ID>.appspot.com/spanner` in a web browser.

Note: by default all the Spanner example operations run in order, so this
endpoint handler may take a while to return.
