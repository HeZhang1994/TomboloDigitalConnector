[![Tombolo](https://user-images.githubusercontent.com/14051876/34300521-c74ced0a-e728-11e7-9f1a-fa233a987750.png)](http://www.tombolo.org.uk/products/)

# Tombolo Digital Connector

[![wercker status](https://app.wercker.com/status/2279bdc90688501386b12c693be6a186/s/master "wercker status")](https://app.wercker.com/project/byKey/2279bdc90688501386b12c693be6a186)

The [Tombolo Digital Connector](http://www.tombolo.org.uk/products/) is an open source tool that enables users to 
seamlessly combine different sources of datasets in an efficient, transparent and reproducible way.  

There are three particularly important parts to the Tombolo Digital Connector:

- [***Importers***](documentation/importers.md)
  - Built-in importers harvest a range of data sources into the centralised data format. Examples include data from ONS, OpenStreetMap, NOMIS, the London Air Quality Network and the London Data Store. **We welcome the creation of additional importers**.
- [***Centralised data format***](documentation/local-datastore.md)
  - All data imported into the Tombolo Digital Connector adopts the centralised data format. This makes it easier to combine and modify data from different sources.
- [***Recipes***](documentation/recipe-language.md)
  - Users generate recipes with a declarative 'recipe language' to combine the data in different ways. This combination can generate new models, indexes and insights. For example, [existing recipes](https://github.com/FutureCitiesCatapult/TomboloDigitalConnector/tree/master/src/main/resources/executions/examples) can generate models of social isolation, calculate the proportion of an area covered by greenspace and even generate an active transport index. **We welcome the creation of additional recipes**.

For further information see the [documentation](documentation/README.md).

## Table of Contents:

- [Contributing](#contributing)
- [Requirements](#requirements)
- [Quick start](#quick-start)
- [Local Deploy](#local-deploy)
- [Run Tasks](#run-tasks)
- [Start/Stop server](#start-stop-server)
- [Implementations](#implementations)
- [License](#license)

![The Challenge](https://user-images.githubusercontent.com/14051876/33429706-cf9edfdc-d5c5-11e7-9cff-f57e9b85f097.gif?raw=true)

## Contributing

Looking to get involved? Have a look at the [Open Source Community milestone](https://github.com/FutureCitiesCatapult/TomboloDigitalConnector/milestone/22) where we have selected *low hanging fruit* for you to easily get involved and contribute.
Read our [Guide to contribution](CONTRIBUTING.md) for details.

## Requirements

To get started you will need to install the requirements to run the Digital Connector.

**Note: you’ll need to have administrator rights on your machine to install these - 
make sure that you do before you proceed.**
  
**Install the following** via the link through to their installation page:

- [Java Development Kit (1.8.x)](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
- [PostgreSQL (9.4+)](https://www.postgresql.org/)
- [PostGIS (2.1+)](http://postgis.net/)
- [Gradle (2.12+)](https://gradle.org/)
- [Git](https://git-scm.com/download/)

After the successful installation of the requirements, you can use the Digital Connector by following the instructions 
in the [quick start](#quick-start) section or by going through the [intro tutorial](/documentation/using-the-digital-connector.md)
in the documentation.

## Installation Guides

- [Windows](documentation/windows-installation-guide.md)
- [macOS](documentation/macOS-installation-guide.md)
- [Ubuntu](documentation/ubuntu-installation-guide.md)

## Quick start

This tutorial will guide you to a quick start on **macOS**.

#### A note about the Terminal

The [Terminal](https://en.wikipedia.org/wiki/Terminal_(macOS)) application can be found in the Applications -> Utilities folder or quickly accessed through Spotlight. It is pre-installed in **macOS** so there is no need to install it.

You will need this application to run some of the commands of this tutorial. When you enter a command and press 
return/enter, the terminal will execute it and complete the task.

**Make sure to press return after typing a command before you enter the next one.**

### Let's start

- Open the **Terminal**. All the following steps will operate in it.

- Get the Digital Connector code to your local machine by cloning its repository.

  ```bash
  git clone https://github.com/FutureCitiesCatapult/TomboloDigitalConnector
  ```

  If successful, you will see a log similar to the below.

  ```bash
  $git clone https://github.com/FutureCitiesCatapult/TomboloDigitalConnector  
  Cloning into 'TomboloDigitalConnector'...
  remote: Counting objects: 15761, done.
  remote: Compressing objects: 100% (184/184), done.
  remote: Total 15761 (delta 90), reused 193 (delta 49), pack-reused 15487
  Receiving objects: 100% (15761/15761), 178.89 MiB | 3.04 MiB/s, done.
  Resolving deltas: 100% (7647/7647), done.
  ```

- Go to the Digital Connector root directory and run `./setup/setup_osx.sh`


If successful the final output will be as the following.

```bash
$ gradle test
:compileJava UP-TO-DATE
:processResources UP-TO-DATE
:classes UP-TO-DATE
:compileTestJava UP-TO-DATE
:processTestResources UP-TO-DATE
:testClasses UP-TO-DATE
> Building 85% > :test > 50 tests completed
:test

BUILD SUCCESSFUL

Total time: 4 mins 50.919 secs
```

**If the tests start to fail then check the PostgreSQL server is running and the requirements are properly installed by
 going through the previous steps.**
 
About to be mentioned a couple of examples of what might have gone wrong in the process if the tests start failing.

```bash
uk.org.tombolo.core.AttributeTest > testUniqueLabel FAILED
    java.util.ServiceConfigurationError
        Caused by: org.hibernate.service.spi.ServiceException
            Caused by: org.hibernate.exception.JDBCConnectionException
                Caused by: org.postgresql.util.PSQLException
                    Caused by: java.net.ConnectException

uk.org.tombolo.core.AttributeTest > testWriteJSON FAILED
    java.util.ServiceConfigurationError
        Caused by: org.hibernate.service.spi.ServiceException
            Caused by: org.hibernate.exception.JDBCConnectionException
                Caused by: org.postgresql.util.PSQLException
                    Caused by: java.net.ConnectException

uk.org.tombolo.core.DatasourceTest > testWriteJSON FAILED
    java.util.ServiceConfigurationError
        Caused by: org.hibernate.service.spi.ServiceException
            Caused by: org.hibernate.exception.JDBCConnectionException
                Caused by: org.postgresql.util.PSQLException
                    Caused by: java.net.ConnectException
```

The former error log is launched if the server is not running and to solve it you need to run the command.

```bash
pg_ctl -D /usr/local/var/postgres -l /usr/local/var/postgres/server.log start
```
OR if you did not set up the tombolo_test database.

In case you see this other error instead, it means that you did not rename the settings files successfully.

```bash
FAILURE: Build failed with an exception.

* Where:
Build file '/TomboloDigitalConnector/build.gradle' line: 159

* What went wrong:
Execution failed for task ':test'.
> Test environment not configured. See the README.
```

If you see other errors, try to go back and follow the steps again.

### Run the Digital Connector

Now you are all set to run a task on the Digital Connector.

The next step is to run an example to show how the digital connector combines different data sets.
We’re using an example that shows the relationship between air pollution (demonstrated in this example by NO2 levels), 
and car and bicycle traffic in every borough in London. You can read more about this example [here](documentation/tutorial-cycling-air-quality.md).  

When you’ve run this example, you can expect a map that looks like this: 

![Final Output](https://user-images.githubusercontent.com/14051876/33429682-b96d18f0-d5c5-11e7-8ca5-b86f0eaa7376.png)

#### To get started:

- Run the following command into the Terminal.

  ```bash
  gradle runExport -Precipe='src/main/resources/executions/examples/london-cycle-traffic-air-quality-lsoa-backoff.json' -Poutput='london-cycle-traffic-air-quality-lsoa-backoff-output.json'
  ```

- You can expect it to take around 1.5 minutes to generate the output, which will be saved in the current directory.
Change the path in the command in case you want it saved elsewhere.

  The output will look similar to the next content:
  
  ```json
  {
    "type":"FeatureCollection",
    "features":[
      {
        "type":"Feature",
        "geometry":{
          "type":"Polygon",
          "coordinates":[[[-0.0802,51.5069],[-0.1092,51.5099],[-0.1114,51.5098],
                          [-0.1116,51.5153],[-0.1053,51.5185],[-0.0852,51.5203],
                          [-0.0784,51.5215],[-0.0802,51.5069]]]
        },
        "properties":{
          "label":"E09000001",
          "name":"City of London",
          "Nitrogen Dioxide":81.3333333333333,
          "Bicycle Fraction":0.25473695591455 
        }
      }, ...
      ...
    ]
  }
  ```
  
- Once you have your output, you can open with a geospatial visualisation tool. For this example, we recommend QGIS,
 and [here](/documentation/open-with-qgis.md) you can find a guide on how to use it.

**We need your feedback!  
If you have any issues with setting up the tool, or running the tutorial, or if you have some advice about how we can 
do this better, please contact us by creating an [issue](https://github.com/FutureCitiesCatapult/TomboloDigitalConnector/issues). 
Our goal is for someone to get back to you within 24 hours.**

#### See also:

- [Learn more about the **example** used in this tutorial](documentation/tutorial-cycling-air-quality.md)
  
- [Use other examples to trail the Digital Connector](src/main/resources/executions/examples)

- [Understand the structure of the recipe](https://github.com/FutureCitiesCatapult/TomboloDigitalConnector/files/1548320/annotatedRecipe.pdf)

- [Learn how to build your own recipe](documentation/recipe-language.md)

- [Learn more about the UK geographic boundaries (LSOA, MSOA, Ward, etc) we often refer to in our tutorials and
recipes.](https://census.ukdataservice.ac.uk/use-data/guides/boundary-data)

### Run tests

```bash
gradle test
```

If you use the IntelliJ JUnit test runner, you will need to add the following to your
VM Options in your JUnit configuration (Run -> Edit Configurations -> All under JUnit,
and Defaults -> JUnit):

```
-enableassertions
-disableassertions:org.geotools...
-Denvironment=test
-DdatabaseURI=jdbc:postgresql://localhost:5432/tombolo_test
-DdatabaseUsername=tombolo_test
-DdatabasePassword=tombolo_test
```

## Local deploy

To deploy to your local Maven installation (`~/.m2` by default):

```bash
gradle install
```

## Run Tasks

### Run export

We use the Gradle task `runExport` to run exports. The parameters are as follows:

```bash
gradle runExport -Precipe='path/to/spec/file.json' -Poutput='output_file.json' -Pforce='com.className' -Pclear=true
```

For example, this calculates the proportion of cycle traffic received at a traffic counter relative to the total traffic
in a given borough and outputs the results to the file `reaggregate-traffic-count-to-la.json`:

```bash
gradle runExport -Precipe='src/main/resources/executions/examples/reaggregate-traffic-count-to-la.json' -Poutput='reaggregate-traffic-count-to-la_output.json'
```

### Export data catalogue

We use the Gradle task `exportCatalogue` to export a JSON file detailing the capabilities of the connector
and explore the data catalogue.

```bash
gradle exportCatalogue -Poutput=catalogue.json
```

### Importer Info

We use the Gradle task `info` to get details about a specific importer

```bash
gradle info
```
would give you list of all the Importers available in Digital Connector

```bash
gradle info -Pi='uk.org.tombolo.importer.dft.TrafficCountImporter'
```
Lists all the details of the Importer like Provider, SubjectTypes, Attributes, Datasourceids, Dataurl

```bash
gradle info -Pp -Pi='uk.org.tombolo.importer.dft.TrafficCountImporter'
```
would give user Datasourceids, Dataurl and Provider. Other option like -Pa and -Ps will give Attributes and SubjectType respectively.

**Note:** Datasourceids and Dataurl will always be provided irrespective of the option given.

## Start/Stop server

If you need to start or stop the server (on MacOS X), use the following commands.

```bash
# to start
pg_ctl -D /usr/local/var/postgres -l /usr/local/var/postgres/server.log start

# to stop
pg_ctl -D /usr/local/var/postgres -l /usr/local/var/postgres/server.log stop
```

## Implementations

- [Python](https://github.com/FutureCitiesCatapult/digital-connector-python)

## License

[MIT](LICENSE)

When using the Tombolo or other GitHub logos and artwork, be sure to follow the [GitHub logo guidelines](https://github.com/logos).
