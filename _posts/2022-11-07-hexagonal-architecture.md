# Hexagonal Architecture
# To start with. A software architecture is what?

Set of patterns and decisions that provide a necessary frame of reference to guide the construction of software, allowing developers to work in the same line and be focused on business decisions.  

It will define the structure, operation and interaction between the parts of the software.

# WHY?


- **Maintainable code:** Reduce the time needed to maintain and modify your code over time by writing maintainable code. A general rule of thumb is that maintainability increases with decreasing technical debt. (Reduce/Avoid Accidental Complexity) This means that we can produce value as quickly as the necessary complexity permits.
![complexity](https://user-images.githubusercontent.com/17130699/200449425-8acc1a37-6569-4706-964e-d54764f994f5.png)

- **Simple and easy to code:** The code flow is quite similar among the features that we have to develop. We have less cognitive load and we can focus more on business logic.
- **Separation of concerns:** Because our business logic is independent, the domain only depends on itself. The code is also more organized.
- **Flexible and adaptable (Ready for Changes)**: Through contracts, also known as interfaces, the application's fundamental business logic communicates with the other components. As a result, we are able to have multiple implementations for a certain contract and switch between them as necessary.
- **Testing Capability**: For each layer, tests can be created. It is always simpler to test independently; if necessary, we may fake any dependencies. resulting in quicker tests and the confidence to introduce a new feature.
- **Easy to onboard → software economics**

# WHAT?

The clean architecture was introduced by Uncle bob. It's the organization of our code so it's easy to understand and easy to change as the project grows.

## How do clean architectures achieve this?

This architectural pattern directs us in using layers and one golden dependency rule to structure our project.

![golden-rul](https://user-images.githubusercontent.com/17130699/200449494-93182426-fc79-4ae1-ac21-86bb438ef5bc.png)


## Golden rule

From the outside layer, we can only know the next layer. And from inside we can not know the outside layer. Therefore: **Infrastructure → Application → Domain**

## Let's talk about parts

## Layers

### Domain

It's where we design our business logic and define our contracts with the outside world (infrastructure)

### Application

This is where the use case lives. Sign-In, Sign-Up, Create User, etc

### Infrastructure

Is the code that changes based on external decisions. Is the layer where the implementation of the interfaces is defined in the domain layer. We are going to rely on the SOLID DIP to be able to decouple from external dependencies.

## Code flow

![code-flow](https://user-images.githubusercontent.com/17130699/200449457-6392b2b0-c80d-4df6-909d-465323ccc280.png)


## Testing

This is one possible strategy to test:

- Unit tests: Application layer and domain
    - We can validate the business logic of our use cases and see if everything behaves as expected.
    - Fast to execute. We are going to mock every infrastructure component.
    - We don't care about the inputs/outputs of our application. We are going to execute the use case directly.
    - Since our fast and easy to develop, in these tests we place the most exhaustive checks regarding the different ramifications of our use cases.
- Integration test: Infraestructure
We test that the real infrastructure works as expected separately.
- Acceptance test: Once we know that the Infrastructure, Application and domain are working right we test it working together.

# Application Services

They **atomically** represent a use case for our system. In case of modifications to the state of our application: 

- You can make the transactional barrier with the persistence system.
- They will publish the respective domain events.
The application service is a coordinator. It does by calling the differents dependencies required to carry out a use case.

# Entity

Objects have a unique identity that transcends time and many representations. These are referred to as "reference objects" as well. (MUTABLE)

# Value Objects

The same as an entity BUT lacks the “identity” AND is IMMUTABLE.(Immutable means that can not be changed)

- Pros:
    - **Semantics (domain)**: Having our domain concepts makes the code easier to comprehend and more effectively represents our domain notions.
    - **Cohesion**: When we model all the logic referring to the same object, our code will be focused on what it should be doing
        - **Avoid redundant checks:** We only need to visit one location to check something related to a value object. We want to prevent having to duplicate the code everywhere this value object might be used.

# Domain Events

An event is something that has happened in the past. A domain event is something that happened in the domain that you want other parts of the same domain (in-process) to be aware of. The notified parts usually react somehow to the events.

Pros: 

- (OP) Open-Closed principle
- SRP (Single Responsibility Principle)
- Decouple teams (having teams that are independent of each other).
