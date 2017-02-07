#Page title

## Introduction
*This part should explain the overall goal of the feature*

### Goal and Scope

*Discuss the scope of the document and how it accomplishes its purpose.*


### Assumptions & Constraints

*Provide a list of assumptions and/or constraints that are preconditions to the preparation of the specification document.*

*Assumptions are future situations beyond the control of the project, whose outcomes influence the success of a project. Constraints are boundary conditions on how the system must be designed and constructed.  Examples include legal requirements, technical standards, strategic decisions.*

## Product Marketing Messaging Framework

For           | (product, solution, offering)
:------------ |:-------------
              | *Specify the product(s) the feature should be in e.g.: eZ Platform, eZ Studio, eZ Personalization Service...*
**What**      | (purpose)
              | *Explain the purpose of the feature*
**How**       | (what does it do / differentiators)
              | *Explain how it is done, how it differentiate from other way to do*
**Who**       | (target audience)
              | *Define the persona(s) that will use the feature*
**Why**       | (value proposition / business value)
              | *Identify business value, this will help product marketing*              


## Functional Specifications -  User stories
*Identify here the user stories that we want to make possible, group them by priority. We can go with only the MVP but it is good to have some perspective on what else could come.*
*Dont forget user story related to extension and integration, such as "As a developer I want to be able to create my custom field type".*

### User Story subset 1 - minimum VALUABLE product

User Story ID | User Story Description
:------------ |:-------------
1             | ``` As a user I want to do something with the software ```
2             | ``` As a user I want to do something else with the software ```


### User Story subset n - minimum VALUABLE product

User Story ID | User Story Description
:------------ |:-------------
3             | ``` As a user I want to do more thing with the software ```
4             | ``` As a user I want to do even more thing with the software ```


## Functional Specifications -  Detailed behaviors
*Specify here the detailed behaviors using user interface wireframe, business logic specification, business logic, error handling, messages. Behavior should as much as possible come with identified behavior scenario to develop and test, using BDD.*

### User story n - As a user I want to do xyz
*detailed specifications and wireframe here > We should aim for a set of BDD at the end for each user story.*

BDD ID  | BDD Scenario
:------ |:-------------
3       | Given [Context] <br /> When [Event Occur] <br /> Then [Outcome]
4       | Given [Context] <br /> When [Event Occur] <br /> Then [Outcome]


## Non-Functional Requirements
### Performance / Scalability / Caching
*Add here any consideration on performance*

### System Design
*Add here any consideration on system design*

### Locales / Translations
*Add here any consideration on localization*

### Usability / GUI
*Add here any consideration on Usability*

### Security
*Add here any consideration on Security*

### Documentation
*Add here any consideration on Documentation*
