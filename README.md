# Fearless Monolith to Microservices Migration – A guided journey

![ticketmonster](./assets/ticketmonster.png)

# :warning: This repository is currently not maintained and will be archived soon. If you are still using it, please reach out to the maintainers :warning:

This repository is a clone from [ticket-monster-msa/monolith](https://github.com/ticket-monster-msa/monolith) maintained by Christian Posta, who gave us kind permission to reuse it for our cloud migration showcase.

The repository is a monorepo of projects that illustrate migrating a monolith application (TicketMonster) to microservices on OpenShift. For this journey, a blog post series explains the required concepts and best practices. Find background information and instrucitons how to use this repo in our [Fearless Monolith to Microservices Migration – A guided journey](https://www.dynatrace.com/news/blog/fearless-monolith-to-microservices-migration-a-guided-journey/) blog series that guides you through the different stages in a structured manner. 

## Overview

This repository consists of a couple of sub-projects used to illustrate a migration from a JAVE EE monolithic application ([Ticketmonster](https://github.com/ticket-monster-msa/monolith)) to microservices living in the OpenShift platform.

### Dynatrace OneAgent Operator
Apply the [Dynatrace OneAgent operator](./dynatrace-oneagent-operator/) on your cluster to have full insights and monitoring enabled for your complete OpenShift environment. Find more information in the blog post here: https://www.dynatrace.com/news/blog/introducing-dynatrace-oneagent-operator/ 


### monolith
The getting started experience begins with the [monolith](./monolith) project. In this project we deploy our monolith application and understand the domain, architecture, and structure of the application that will be the foundation for successive iterations.
 
### monolith-proxy
A simple proxy webserver that is in front of the monolithic TicketMonster and can serve for different purposes. In our example, it simply redirects traffic to and from the TicketMonster.

### tm-ui-v*
The `tm-ui-*` folders contain different versions of the front-facing UI that we use as we migrate from a monolith to split out the UI to the set of microservices.

The [tm-ui-v1](./tm-ui-v1/) folder contains a version of the front-facing UI that we use as we migrate from a monolith to split out the UI to the set of microservices.

The [tm-ui-v2](./tm-ui-v2/) folder contains a version of the front-facing UI that we use to communicate with the [backend-v1](./backend-v1/) instead of the monolith.

### backend-v*
The `backend-*` folders contain the monolith with the UI removed and successive iterations of evolution. With `backend-v1`, we have taken the monolith as it is and removed the UI. It contains a REST API that can be called from the UI. In `backend-v2` we've added feature flags for controlling the introduction of a new microservice. See each respective sub project for more information.

### orders-service
Tihs subproejct containts the first microsercive that is extracted from the monolith. The [orders-service](.orders-service/) does communicate with the `backend-v2` and the data flow is controlled via FF4J. 



## Instructions

1. Clone the repository
   ```
   $ git clone https://github.com/dynatrace-innovationlab/monolith-to-microservice-openshift.git
   ```
1. Lift-and-shift TicketMonster to OpenShift
  
   In directory `monolith`, follow the [Instructions](./monolith/) in the readme to run TicketMonster on OpenShift.

1. Set a new UI in front of TicketMonster

   In directory `tm-ui-v1`, follow the [Instructions](./tm-ui-v1/) in the readme to extract the UI from the TicketMonster. 

   ![tm-ui-v1](./assets/tm-ui-v1.png)

1. Get rid of the legacy UI in the monolithic code base
    
    Deploy `tm-ui-v2` that is set up to communicate with `backend-v1`. Follow the [instruction here](./tm-ui-v2/).

    
   ![tm-ui-v2](./assets/tm-ui-v2.png)



1. Deploy the first microservice

    Deploy `backend-v2` which will communicate with the `orders-service` as your first microservice. Follow the [instructions here](./backend-v2/).

    ![tm-orders-service](./assets/tm-orders-service.png)