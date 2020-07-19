## BEHAVIOR SPECS’ SPECS

## Introduction to Behavior Driven Development

Software development and project management is general traditionally focuses on a plan for success. This is not good enough for mission-critical and civilization scale software that also must plan for failure and for moving from failure or incompleteness to success or release regularly. 

Initially, [covidsim.team](https://www.covidsim.team/)’s dashboard (user interface for data visualization and inputs) and related systems like our self-hosted cloud drive (collaborative editors and storage for Word, Excel, PowerPoint, Images) and supercomputer management software (custom VPS manager) were meant only for internal usage within the research team. And since I was the only one working on the system since 2015, I did not find it necessary to formalize my software development process with a development team in mind.
 
Now that [covidsim.team](https://www.covidsim.team/) is committed to testing the usage of some newly created forms by our public health team using this platform, we are going to need a proper development process for this national scale ambition. This BDD-based process is applicable for any quadrant in the [Cynefin model](https://hbr.org/2007/11/a-leaders-framework-for-decision-making).


![R0711C_A.gif](https://cdn.hashnode.com/res/hashnode/image/upload/v1595142789225/gEK9TuyUK.gif)

These forms and visualizations of related data that we are about to roll out are quite trivial parts of the underlying system, technically speaking. However, these are the primary way for any user to interact with our COVID-specific system components currently and they effectively form a portal to the rest of my collaborative systems. Hence it is vital to us to define the behavioral specifications for each of the newly designed forms, data files or visualizations. Without a clear consensus on the new behavior, the new feature cannot - and therefore will not - be made. These specifications will be stored on cosys.work's Nepali servers only. We will require approvals and editing from a single point of contact from the customers and me or a software lead we appoint in the future. Updates to the documents should be notified via emails.

### BDD Examples, Links and Resources

From: [blogs.harvard.edu/markshead](https://blogs.harvard.edu/markshead/what-is-behavior-driven-development/) 

A BDD feature file consists of one or more scenarios. These scenarios are just examples of how the application should behave from the standpoint of a user. For example, you might have a scenario that says:


> Given I am on the login page

> When I attempt to login with valid credentials

> Then I am shown the application dashboard

And another scenario that says:

> Given I am on the login page

> When I attempt to login with invalid credentials

> Then I am shown a login error message

These examples are written in English, but are still very structured. The Given portion tells the starting condition for the example, the When line tells what you actually do, and the Then tells what results are expected. 
____________________________________________________________________________________________________

More formally speaking, the given clause is there to define the pre-conditions and context for a new feature. Similarly, the when clause is there to define the user action or a software event that may need a reaction or response from the system. Finally, the then clause is there to describe the reaction, response or output expected from the system for this [scenario](https://automationpanda.com/2017/01/30/bdd-101-writing-good-gherkin/).

Another thing to note is that these Given-When-Then behavior specifications are not just documentational or prescriptive. They become a part of the source code of the software. The modules that have these specifications only perform what is specified in the Given-When-Then specs - nothing more, nothing less. These specifications and the software itself may change as frequently as the real-world requirements of the software change e.g. every second or week. 

### User Story -> Agreement -> Implementation

From:  [cucumber.io](https://cucumber.io/docs/bdd/) 

A user story is a story created about how a user might use the system. These are usually sent over to the development team by the customer organization or written by the project manager. This will typically consist of a few Given-When -Then sentences with logical connectives like 'and', 'or', 'not', 'with', 'but', 'once' or 'never'.

For example, our first user story could be about how someone from a municipality office logs in to her/his municipality’s system and what pages and data the user could expect to see and use. 

Another user story we might need soon after launch is the story that imagines and describes how a user from EDCD/NPHL/NHRC/CCMC/MOHP could upload to the system. In this case, we may assert in the given clause that the user is already logged in. We could also create a user persona e.g. Biplav is a non-technical user at EDCD. So, this story could initially look like this:

> **Given** Biplav is logged in to the dashboard

> **When** Biplav clicks the CSV file uploader in the left sidebar

> **Then** Biplay should be able to select, preview, review, upload **or** cancel the upload 

These kinds of user stories may need further breakdown. This breakdown itself will also consist of more GWT sets. For example, this GWT set further explains what the system should do:

> **Given** Biplav has selected a CSV file to upload to the database using the file uploader **and** Biplav has not yet confirmed the upload

> **When** The system informs him of any mistakes e.g. how the CSV headers are labeled [+as many examples as possible]

> **Then** Biplay should be able to edit the CSV headers **and/or** data before he cancels **or** confirms upload of the data.

![bdd-practices-diagram.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1595142730242/xAIf2p5vm.png)

How the system **could** behave is a crucial part of the development process. This is the **discovery phase** and is typically gathered via interactive sessions and workshops with some of the potential users of the system. The discovery phase allows for surfacing and formalizing organizational rules and a shared understanding. 

An even more important part of development is the finer details of how the system **should** behave. This is done in the **formulation phase**. We create specifications, mocks, wireframes and break user stories into feasible epics or tasks in this phase.

**Automation** phase is about how the system actually behaves. Validation and demonstration of each automated process may lead to further discoveries and formulations.
