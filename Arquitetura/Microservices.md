# Microservices

*However, if you always remember that the most important idea behind microservices is the need to decompose your application into smaller logical pieces, you are already halfway there*.

## What are Microservices?

Microservices are maybe a good approach to solve problems; in the last few years, companies such as Netflix, PayPal, eBay, Amazon, and Spotify have chosen to use microservices in their own development teams because they believed them to be the best solution for their projects. To understand why they chose microservices and understand the kinds of projects you should use them in, it is necessary to know what a microservice is.

Firstly, it is essential to understand what a monolithic application is, but basically, we can define a microservice as an extended Service Oriented Architecture. In other words, it is a way to develop an application by following the required steps to turn it into various little services. Each service will execute itself and communicate with others through requests, usually using APIs on HTTP.

It is important to differentiate between MVC and microservices. MVC is a design pattern and microservices are a way to develop an application;

A microservice is a simple, isolated entity with a concrete proposal. It is independent and works with the rest of the microservices by communicating through an agreed channel.

Another advantage of using microservices is that they are agnostic to the language; in other words, it is possible to use different languages for each microservice. A microservice written in PHP can talk to others written in Python or Ruby because they only give the API to the rest of the microservices, so they just have to share the same interface to communicate with each other.

### Microservices characteristics

Next, we will look at the key elements of a microservice architecture:

* **Ready to fail**: Microservices are designed to fail. In a web application, microservices communicate with each other and if one of them fails, the rest should work to avoid a cascading failure. Basically, microservices attempt to avoid communicating synchronously using async calls, queues, systems based on actors, and streams instead. This topic will be discussed in the subsequent chapters.

* **Unix philosophy**: Microservices should follow the Unix philosophy. Each microservice must be designed to do one thing only, and should only be small and independent. This allows us as developers to adjust and deploy each microservice independently. The Unix philosophy emphasizes building simple, short, clear, modular, and extensible code that can be easily maintained and repurposed by developers as well, in addition to its creators.

* **Communication layer**: Each microservice communicates with the others through HTTP requests and messages, executing the business logic, querying the database, exchanging messages with the required systems and, at the end, returning a JSON (or HTML/XML) response.

* **Scalability**: The main reason to choose a microservice architecture is that it is possible to scale the application easily. The bigger an application is and the more traffic the application has, the more sense the proper selection of choosing microservices makes. A microservice can scale the required part without any impact on the rest of the application.

### How to focus your development on microservices

#### Disadvantages of microservices

Next, we will look at the disadvantages of the microservices architecture. When asking developers about this, they agree that the major problem with microservices is the debugging on the production server.

Debugging an application based on microservices can be a little tedious when you have hundreds of microservices in your application and you have to find where a problem is; you need to spend time looking for the microservice that is giving you the error. Each microservice works like an individual machine, so to look at a specific log you have to go to the specific microservice. Fortunately, there are some tools to help us out with this subject by getting the logs from all the different microservices in your application and putting them together into a single location. In the subsequent chapters, we will look at these kinds of tools.

Another disadvantage is that it is necessary to maintain every single microservice as an entire server; in other words, every single microservice can have one or more databases, logs, different services, or library versions, and even the code can be in a different language, so if it is difficult to maintain a single server and doing it with hundreds will waste money and time.

Also, the communication between microservices is very important---they have to work like clocks, so communication is essential for the application. To do this, the communication between the development teams will be necessary to tell each other what they need and also to write good documentation for each microservice.

A good practice to work with microservices is having everything automatized or at least everything possible. Maybe the most important part is the deployment. If it is necessary to deploy hundreds of microservices, it can be difficult. So, the best way is to automatize these kinds of tasks.

#### Always create small logical black boxes

As a developer, you always start with the big picture of what you will build. Try to decompose the big picture into small logical blocks that only do one thing. Once the multiple small pieces are ready, you can start building complex systems, ensuring that the foundations of your application are solid.

Each of your microservices is like a black box with a public interface, which is the only way to interact with your software. The main recommendation you need to have always in mind is to build a very stable API. You can change the implementation of an API call without many problems, but if you change the way of calling or even the response of this call, you will be in big trouble. In the case of deep changes to your API, ensure that you use some kind of versioning so that you can support the old and new versions.

#### Always think about scalability

One of the main advantages of a microservice application is that each service can be scaled up or down. This flexibility can be achieved by reducing the number of stateful services to the minimum. A stateful service relies on data persistence, making it difficult to move or share the data without having data consistency problems.

Using autodiscovery and autoregistry techniques, you can build a system that knows which one will deal with each request at all times.

#### Use a lightweight communication protocol

Nobody likes to wait, not even your microservices. Don't try to reinvent the wheel or use an obscure but cool communication protocol, use HTTP and REST. They are known by all web developers, they are fast, reliable, easy to implement, and very easy to debug. If you need to increase the security, implement SSL/TSL.

#### Use queues to reduce a service load or make async executions

As a developer, you want to make your system as fast as possible. Therefore, it makes no sense to increase the execution time of an API call just because it is waiting for some action that can be done in the background. In these cases, the best approach is the use of queues and job runners in charge of this background processing.

Imagine that you have a notification system that sends an e-mail to a customer when placing an order on your microservice e-commerce. Do you think that the customer wants to wait to see the payment successful page only because the system is trying to send an e-mail? In this case, a better approach is to enqueue the message so that the customer will have a pretty instant thank you page. Later, a job runner will pick up the queued notification and the e-mail will be sent to the customer.

#### Each service is different, so keep different repositories and build environments

We are decomposing an application into small parts which we want to scale and deploy independently, so it makes sense to keep the source in different repositories. Having different build environments and repositories means that each microservice has its own lifecycle and can be deployed without affecting the rest of your application.

## Domain-driven design

Domain-driven design (DDD from here on) is an approach for the development when it has complex needs. This concept is not new; it was created by Eric Evans in his book with the same title in 2004, but now it is mainstream as microservices are popular among developers and very common in huge projects.

This is happening as there is great compatibility between the microservices concepts (regarding the software architecture, dividing every functionality into services) and DDD concepts (about the bounded contexts).

Before knowing where and how we can use DDD in our microservices project, it is necessary to understand what DDD is and how it works, so let me introduce you to the main concepts as a summary of this approach.

### How domain-driven design works

Evans introduced some concepts that are necessary to understand to learn how domain-driven design works:

- **Context**:This is the setting in which a word or statement appears that determines its meaning.

- **Domain**: This is a sphere of knowledge (ontology), influence, or activity. The subject area to which the user applies a program is the domain of the software.

- **Model**:This is a system of abstractions that describes selected aspects of a domain and can be used to solve problems related to that domain.

- **Ubiquitous language**:This is a language structured around the domain model and used by all team members to connect all the activities of the team with the software.

The **software domain** is not related to the technical terms, programming or computers in any way. In most projects, the most challenging part is to understand the business domain, so DDD suggests using a **model domain**; this is abstract, ordered, and selective knowledge reproduced in a diagram, code, or just words.

The model domain is like the roadmap to build projects with complex functionalities, and it is necessary to follow five steps to achieve it. These five steps need to be agreed on by the development team and the domain expert:

1. **Brainstorming and refinement**: There should be a communication channel between the development team and the domain expert. So, all the people in the project should be able to talk to everyone because they all need to know how the project should work.
2. **Draft domain model**: During the conversation, it is necessary to start drawing a draft of the domain model, so that it can be checked and corrected by the domain expert until they both agree.
3. **Early class diagram**:Using the draft, we can start building an early version of the class diagram.
4. **Simple prototype**: Using the draft of the early class diagram and domain model, it is possible to build a very simple prototype. Evans suggests avoiding things that are not related to the domain to ensure that the domain business was modeled properly. It can be a very simple program as a trace.
5. **Prototype feedback**: The domain expert interacts with the prototype in order to check whether all the needs are met and then the entire team will improve the model domain and the prototype.

The model, code, and design must evolve and grow together.They cannot be unsynchronized at all. If a concept is updated on the model, it should also be updated on the code and on the design automatically, and the same goes for the rest.

A model is an abstraction system that describes selective concepts of a domain and it can be used to resolve problems related to that domain. If there is a piece of the model that is not reflected in the code, it should be removed.

Finally, the domain model is the base of the common language in a project. This common language in DDD is called **ubiquitous language** and it should have the following things:

- Class names and their functions related to the domain
- Terms to discuss the domain rules included on the model
- Names of analysis and design patterns applied to the domain model

The ubiquitous language should be used by all the members of the project, including developers and domain experts, so the developers should be able to describe all the tasks and functions.

It is absolutely necessary to use this language in all the discussions between the team, such as meetings, diagrams, or documentation, but this language was not born in the first iteration of the process, meaning that it can take many iterations of refactoring having the model, language, and code synchronized. If, for example, the developers discover that a class from the domain should be renamed, they cannot refactor this without refactoring the name on the domain model and the ubiquitous language.

The ubiquitous language, domain model, and code should evolve togetheras a single knowledge block.

There is controversial concept on DDD. Eric Evans says that it is necessary for the domain expert to use the same language as the team, but some people do not like this idea. Usually, the domain experts do not have knowledge of object-oriented concepts or microservices because they are too abstract for non-developers. Anyway, DDD says that if the domain expert does not understand the domain model, it is because there is something wrong with it.

There are diagrams in the domain model, but Evans suggests using text as well, because diagrams do not explain the concepts properly. Also, the diagrams should be superficial; if you want to see more details you have the code for it.

Some projects are affected by the connection between the domain model and the code. This happens because there is a division between analysis and design. The analysts make a model independent of the design and the developers cannot develop the functionalities because some information is missing. In addition, they cannot talk with the domain expert. The development team will not follow the model and, in the end, the domain model will not be updated and it will not work. Therefore, the project will not meet the requirements.

To sum up, DDD works to achieve the software development as an iterative process of refinement of the model, design, and code as a single task in a block.

### Using domain-driver design in microservices

As we said before, DDD meets the microservice's needs perfectly. A common problem with microservices appears because they have decentralized data management; this has advantages but can be problematic sometimes.

The concept model between two services will be different, and it can cause problems in huge companies. For example, a user can differ depending on the service, the attributes for each service regarding the user can differ and, also, the attribute semantic can differ.

It is even more complex when, in a big company, the application evolves a lot and has updates for many years. Each service can have different attributes for the user and, generally, they do not match. So, a great way to solve this is using DDD.

As microservices do, DDD divides a complex domain into different contexts, making relationships between them and asking for the collaboration of all the members to get a ubiquitous language in a particular domain and bounded context, iterating this process until they achieve a real concept regarding the problem.

Evans suggests designing each microservice as a DDD-bounded context so that it will provide a logical boundary for microservices inside a system. Every single microservice (or team working on it) will be responsible for that part of the system, and it will give clearer and maintainable code.

Michael Plöd gave more ideas about how DDD can help microservices. There are four significant areas regarding building microservices:

- **Strategic design**: This is basically bounded context, but context maps and other patterns are important too. A context map should show all the bounded contexts of the project and their relationships with each other; it also describes the contract between them. The context map is very useful for monolithic applications wanting to move into microservices.

- **Internal building blocks**: This refers to using tactical patterns, such as aggregates, entities, or repositories, when designing the inside of a bounded context.

- **Large-scale structures**: This is used to create a structure using evolving order and responsibility layers. This is a concept in microservices too. In huge projects, it is helpful to create large-scale structures into boundary contexts. They should be designed to evolve individually.

- **Distillation**: Distilling a core domain from an already grown system is very useful when migrating a monolithic application into microservices. The most important part should be identifying and extracting the core domain, along with the iteration process of identifying a subdomain, extracting it from the core, and refactoring.

To sum up, microservices and DDD match perfectly, but it is necessary to have a larger scope and understand more than the boundary contexts.
