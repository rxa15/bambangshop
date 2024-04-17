# BambangShop Publisher App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

---

## About this Project
In this repository, we have provided you a REST (REpresentational State Transfer) API project using Rocket web framework.

This project consists of four modules:
1.  `controller`: this module contains handler functions used to receive request and send responses.
    In Model-View-Controller (MVC) pattern, this is the Controller part.
2.  `model`: this module contains structs that serve as data containers.
    In MVC pattern, this is the Model part.
3.  `service`: this module contains structs with business logic methods.
    In MVC pattern, this is also the Model part.
4.  `repository`: this module contains structs that serve as databases and methods to access the databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a basic functionality that makes BambangShop work: ability to create, read, and delete `Product`s.
This repository already contains a functioning `Product` model, repository, service, and controllers that you can try right away.

As this is an Observer Design Pattern tutorial repository, you need to implement another feature: `Notification`.
This feature will notify creation, promotion, and deletion of a product, to external subscribers that are interested of a certain product type.
The subscribers are another Rocket instances, so the notification will be sent using HTTP POST request to each subscriber's `receive notification` address.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Publisher" folder.
This Postman collection also contains endpoints that you need to implement later on (the `Notification` feature).

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    APP_INSTANCE_ROOT_URL="http://localhost:8000"
    ```
    Here are the details of each environment variable:
    | variable              | type   | description                                                |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)
-   ✅ Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   ✅ Commit: `Create Subscriber model struct.`
    -   ✅ Commit: `Create Notification model struct.`
    -   ✅ Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   ✅ Commit: `Implement add function in Subscriber repository.`
    -   ✅ Commit: `Implement list_all function in Subscriber repository.`
    -   ✅ Commit: `Implement delete function in Subscriber repository.`
    -   ✅ Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   ✅ Commit: `Create Notification service struct skeleton.`
    -   ✅ Commit: `Implement subscribe function in Notification service.`
    -   ✅ Commit: `Implement subscribe function in Notification controller.`
    -   ✅ Commit: `Implement unsubscribe function in Notification service.`
    -   ✅ Commit: `Implement unsubscribe function in Notification controller.`
    -   ✅ Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   ✅ Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   ✅ Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   ✅ Commit: `Implement publish function in Program service and Program controller.`
    -   ✅ Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?

In the context of this BambangShop case, we currently only have one type of `Subscriber`, so there is no immediate need for additional interfaces (or traits in Rust). However, if we anticipate future app development where multiple types of subscribers might require different ways of reacting to updates, it would be advisable to use interfaces (or traits in Rust).

2. id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?

DashMap stores data in a key-value format, in contrast to Vec, which employs a list for storage. In the context of the BambangShop case, where `id` in Program and `url` in Subscriber are intended to be unique, using Vec would necessitate manual checks to ensure uniqueness for every inputted `id` and `url`. Therefore, the use of DashMap is recommended here. DashMap inherently manages unique identifiers, ensuring that `id` and `url` inputs remain unique without requiring explicit checks.

3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?

In Rust programming, DashMap is better suited for multithreading. Since Bambangshop utilizes multithreading, DashMap becomes necessary because SUBSCRIBERS will be accessed by multiple threads. Singleton pattern is not applicable here as its purpose is to maintain a single instance of an object throughout the program's execution. However, our focus is on ensuring the safety of data access in a multi-threading scenario. DashMap allows us to securely and efficiently manage the list of subscribers to our products within a single data structure.

#### Reflection Publisher-2
1. In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?

Separating "Service" and "Repository" from a Model is a part of implementing Design Pattern in our code. Following the Single Responsibility Principle (SRP) in SOLID Principle, Service and Repository need to be distinguished due to their distinct responsibilities. Service primarily manages the business logic of the application, while Repository focuses on data storage handling. This separation will significantly enhance the project's maintainability.

2. What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?

If we only use the Model, there are some problems that will arise. For instance, code coupling and complex business logic in models. Imagine if we only have Program Model, Subscriber Model, and Notification Model. Program Model would need to handle not only the core logic related to program management but also the details of data retrieval and storage. Subscriber Model would need to manage both subscriber-related business logic (such as subscription management, notification preferences) and data access logic. Notification Model would need to handle the generation and sending of notifications, as well as interacting with the underlying data storage.

3. Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.

Yes, I have explored deeper into Postman since taking the Platform Based Programming (PBP) class last semester. It assists me in testing the functionality of my program's endpoints. I firmly believe Postman is valuable for testing, as it simplifies the process of testing HTTP responses by sending requests to the developing API. Moreover, Postman provides advanced features like Collaborative Collections, which consolidate multiple HTTP requests, thus facilitating coordination during the development and testing stages of group projects' APIs.

#### Reflection Publisher-3
1. Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and Pull model (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?


2. What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)


3. Explain what will happen to the program if we decide to not use multi-threading in the notification process.
