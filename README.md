# Exercise 3: Web Ontologies and Knowledge Graphs
This repository contains:
- A partial implementation of a Web ontology for the domain of Agriculture based on the [SAMOD methodology](https://essepuntato.it/samod/). 
- A partial implementation of a [JaCaMo](https://jacamo-lang.github.io/) application, in which autonomous agents are able to query a knowledge graph
to retrieve knowledge about the world — and to use that knowledge to manage a farm.

## Table of Contents
- [Exercise 3: Web Ontologies and Knowledge Graphs](#exercise-3-web-ontologies-and-knowledge-graphs)
  - [Table of Contents](#table-of-contents)
  - [Project structure](#project-structure)
  - [Task 2](#task-2)
    - [Task 2.1](#task-21)
    - [Task 2.2](#task-22)
    - [Task 2.3](#task-23)
  - [How to run the project](#how-to-run-the-project)
    - [1. Run the mockserver that mocks the tractors' HTTP APIs](#1-run-the-mockserver-that-mocks-the-tractors-http-apis)
    - [2. Run the JaCaMo application](#2-run-the-jacamo-application)

## Project structure
```
├── SAMOD // the only directory required for Tasks 2.1 and 2.2
│   └── agriculture-domain 
│       ├── README.md // the description of the Motivating Scenario, including the Competency Questions and the preliminary Glossary of terms
│       ├── tbox.ttl // the preliminary TBox that implements the description of the terms in the glossary
│       ├── abox.ttl // the preliminary ABox that implements the description of the first 3 competency questions
│       └── queries  
│           ├── insert // queries for populating a knowledge graph based on the ABox
│           └── select // queries for testing the knowledge graph based on the competency questions
│ // the rest are required for Task 2.3
├── mockserver
│   └── tractors.json // the configuration file for the mockserver that mocks the HTTP APIs of farm tractors 
├── src
│   ├── agt
│   │   ├── irrigator.asl // agent program of the agent that irrigates the soil
│   │   └── moisture_detector.asl // agent program of the agent that detects the moisture level of soil
│   └── env
│       ├── farm
│       │   └── FarmKG.java // artifact that can be used for querying a GraphDB repository
│       └── wot
│           └── ThingArtifact.java // artifact that can be used for interacting with W3C Web of Things (WoT) Things
├── task.jcm // the configuration file of the JaCaMo application for Task 2.3
└── wot-td-java // library for the W3C Web of Things (WoT) Thing Description (TD)
```

## Task 2 
### Task 2.1
- Extend the [Glossary of terms](SAMOD/agriculture-domain/README.md) to define new terms that are required for describing the Motivating scenario and for defining its Competency questions
- Update the [TBox](SAMOD/agriculture-domain/tbox.ttl) to formally define your new terms in an ontology. You can import the TBox in [Protege](https://protege.stanford.edu/) to work on top of the base ontology

### Task 2.2
- Update the [ABox](SAMOD/agriculture-domain/abox.ttl) to complete the formal description of the motivating scenario
  - TIPS: 
    - Use the new terms that you defined in your TBox
    - Currently, the ABox implements the description of the motivating scenario that is required for solving the first 3 competency questions
- Setup your [GraphDB](http://localhost:7200/) repository for testing your ABox and TBox:
  - Import your TBox file in your repository: `Import` > `RDF` > `Upload RDF files`
  - Insert your ABox statements in your repository by creating and executing SPARQL INSERT queries
    - TIP: The [queries/insert](SAMOD/agriculture-domain/queries/insert) directory contains example INSERT queries
  - Create and execute in your repository SPARQL SELECT queries that formalize the competency questions 
    - TIP: The [queries/select](SAMOD/agriculture-domain/queries/select) directory contains example SELECT queries that formalize the first 3 competency questions 

### Task 2.3
- Modify the template to use repository name in the Java class [`FarmKG`](src/env/farm/FarmKG.java), and the agent programs [`moisture_detector.asl`](src/agt/moisture_detector.asl#L4), [`irrigator.asl`](src/agt/irrigator.asl#L4)
- Complete the methods of the Java class [`FarmKG`](src/env/farm/FarmKG.java) that enable autonomous agents to query your GraphDB repository through a JaCaMo application:
  - [`queryFarmSections()`](src/env/farm/FarmKG.java#L127)
  - [`querySectionCoordinates()`](src/env/farm/FarmKG.java#L136)
  - [`queryCropOfSection()`](src/env/farm/FarmKG.java#L145)
  - [`queryRequiredMoisture()`](src/env/farm/FarmKG.java#L154)
- TIPS:
  - Use your SPARQL SELECT queries from Task 2.2 that formalize the competency questions q4, q5, q6, and q7.
  - The class [`FarmKG`](src/env/farm/FarmKG.java) contains the full implementation of the example methods `queryFarm()` and `queryThing()`

## How to run the project
### 1. Run the mockserver that mocks the tractors' HTTP APIs

Run [MockServer](https://www.mock-server.com/) with [Docker Compose](https://docs.docker.com/compose/):
   ```
   docker compose up
   ```
The above command will run two Docker containers, one for the mockserver and for the GraphDB. The latter will be available on http://localhost:7200 shortly afterwards. You can import the [Postman collection](smart-farm.postman_collection.json) by drag and drop the file into your workspace on [postman](https://www.postman.com/) to inspect the behavior of the tractors' mockserver.


### 2. Run the JaCaMo application

You can run the project directly in Visual Studio Code or from the command line with Gradle 8.6.
- In VSCode:  Click on the Gradle Side Bar elephant icon, and navigate through `GRADLE PROJECTS` > `exercise-3` > `Tasks` > `jacamo` > `task`.
- On MacOS and Linux run the following command:
```shell
./gradlew task
```
- On Windows run the following command:
```shell
gradle.bat task
```




