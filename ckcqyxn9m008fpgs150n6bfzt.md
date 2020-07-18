## Socioware Specs: High Hopes

## a. Bottoms Up

The following article is a follow up article about [covidsim.team](https://www.covidsim.team)'s [call for collaboration](https://risav.dev/covid-19-simulation-for-nepal-a-call-for-collaboration-ck85cqi1l0089zbs1bhjyawxd). Contributing to the software systems will be easier once we have a common specification and description of the systems in place. 

> Encumbered forever by desire and ambition

> There's a hunger still unsatisfied

> Our weary eyes still stray to the horizon

> Though down this road we've been so many times

> - Pink Floyd, *High Hopes*


### 1. UX and Services

User interfaces and interaction patterns of this system are built orthogonally and in bulk. Because of this we are able to create new user interface elements using component factories and resolvers. We are able to do this on the fly instead of handcrafting them each time there is a new emergent use case. Therefore, most of this document is dedicated to the rest of the 6 layers in our octal system in which two UI/UX systems cover user inputs and visualizations. 

The user experience is designed with a decentralized burden of responsibility. Every interface and API response type describes its own visualizations and pipelines. Hence simply streaming them through a UI component resolver is enough to get a new feature displayed to the user for use. This only defines our time to user. Our time to user interaction is however still dependent on how fast we fetch all the interaction endpoint services.

### 2. CouchDB and prox-e

The client is a light weight UI and windowing system for ambient computing. It also powers VR/AR interactions apart from Smart TV and Smart phone interfaces. The NoSQL database provides all the necessary data to the local system. All data processing APIs for CouchDB data is able under Potato API provided by a `prox-e` ubiquitous server instance. 

 2,50,00,000 (25 KK) pouch instances, 753 couch instances and 7 potato instances will be needed for an optimal run in our system launch for Nepal. We are possibly starting with Lalitpur Municipality in the central Kathmandu valley and in Karnali remote province. 

### 3. Redis and pub-sub

More intensive applications aside from the dashboard will be needed for other spreading disasters and calamities. For example, for flood-detection we need to be monitoring weather patterns and running the stream against our climate knowledge base pipelines. Similarly, an earthquake response system will guide us out of any building and towards safety only if we provide our software system with the authentic and real time 3D API of our surrounding reality. Since every concept in the system has its own data streams and behavior sets for any given context, it is easier to just fan-out using pub-sub and close-in using topics and filters. 

Each topic has associated filtering processes related to identity, auth, context and content. Each component has multiple topics of linked data. A higher order component handles many such components including lazy loading components as needed. Our dashboard is capable of providing a windowing system overlaid on a map or some other 3D surface because every user experience of the system is channeled through a collection of components which in turn are a collection of pub-sub topics.

### 4. State and Data

Our applications on their own and in integration have different system states. This data needs to be available and persisted. Without the ability to freeze and resume, we cannot really address all possible failure scenarios. Our systems use state and data management to travel back and forth in time and test itself in parallel even when it is being used. 

Because fuzzers and testers of this system continually emulate all possible states and data interactions - using actions, effects and reducers, we are able to have high confidence in the system. And in the new parts of the system that the overall system creates for itself based on the results of those tests and fuzzers. 

## b. Reactive Components

### 1. Abstract Components

### 2. Auth & Context UI filters

### 3. UI Factories & Resolvers

### 4. Labels & Processes


## c. NgRx Breeze

### 1. rx rsmq

### 2. Hibernate & Spark

### 3. Git-like Command Queues

### 4. Being & Performance


## d. Top Down

### 1. Concept

### 2. Relation

### 3. Knowledge

### 4. Representation
