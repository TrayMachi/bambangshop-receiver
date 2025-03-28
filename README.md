# BambangShop Receiver App
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
4.  `repository`: this module contains structs that serve as databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a Rocket web framework skeleton that you can work with.

As this is an Observer Design Pattern tutorial repository, you need to implement a feature: `Notification`.
This feature will receive notifications of creation, promotion, and deletion of a product, when this receiver instance is subscribed to a certain product type.
The notification will be sent using HTTP POST request, so you need to make the receiver endpoint in this project.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Receiver" folder.

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    ROCKET_PORT=8001
    APP_INSTANCE_ROOT_URL=http://localhost:${ROCKET_PORT}
    APP_PUBLISHER_ROOT_URL=http://localhost:8000
    APP_INSTANCE_NAME=Safira Sudrajat
    ```
    Here are the details of each environment variable:
    | variable                | type   | description                                                     |
    |-------------------------|--------|-----------------------------------------------------------------|
    | ROCKET_PORT             | string | Port number that will be listened by this receiver instance.    |
    | APP_INSTANCE_ROOT_URL   | string | URL address where this receiver instance can be accessed.       |
    | APP_PUUBLISHER_ROOT_URL | string | URL address where the publisher instance can be accessed.       |
    | APP_INSTANCE_NAME       | string | Name of this receiver instance, will be shown on notifications. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)
3.  To simulate multiple instances of BambangShop Receiver (as the tutorial mandates you to do so),
    you can open new terminal, then edit `ROCKET_PORT` in `.env` file, then execute another `cargo run`.

    For example, if you want to run 3 (three) instances of BambangShop Receiver at port `8001`, `8002`, and `8003`, you can do these steps:
    -   Edit `ROCKET_PORT` in `.env` to `8001`, then execute `cargo run`.
    -   Open new terminal, edit `ROCKET_PORT` in `.env` to `8002`, then execute `cargo run`.
    -   Open another new terminal, edit `ROCKET_PORT` in `.env` to `8003`, then execute `cargo run`.

## Mandatory Checklists (Subscriber)
-   [X] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [X] Commit: `Create Notification model struct.`
    -   [X] Commit: `Create SubscriberRequest model struct.`
    -   [X] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [X] Commit: `Implement add function in Notification repository.`
    -   [X] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [X] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 3: Implement services and controllers**
    -   [X] Commit: `Create Notification service struct skeleton.`
    -   [X] Commit: `Implement subscribe function in Notification service.`
    -   [X] Commit: `Implement subscribe function in Notification controller.`
    -   [X] Commit: `Implement unsubscribe function in Notification service.`
    -   [X] Commit: `Implement unsubscribe function in Notification controller.`
    -   [X] Commit: `Implement receive_notification function in Notification service.`
    -   [X] Commit: `Implement receive function in Notification controller.`
    -   [X] Commit: `Implement list_messages function in Notification service.`
    -   [X] Commit: `Implement list function in Notification controller.`
    -   [X] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1
> In this tutorial, we used RwLock<> to synchronise the use of Vec of Notifications. Explain why it is necessary for this case, and explain why we do not use Mutex<> instead?
In this tutorial, RwLock<> is used to synchronize access to the Vec of Notifications because it allows multiple readers or a single writer at a time. This is particularly useful when you have a scenario where read operations are frequent, and write operations are less frequent. By using RwLock<>, you can achieve better performance compared to Mutex<>, as Mutex<> only allows one thread to access the data at a time, regardless of whether it is a read or write operation.

> In this tutorial, we used lazy_static external library to define Vec and DashMap as a “static” variable. Compared to Java where we can mutate the content of a static variable via a static function, why did not Rust allow us to do so?
Rust does not allow direct mutation of static variables like Java does because of its strict ownership and borrowing rules, which ensure memory safety and prevent data races. Unlike Java, where static variables can be freely mutated (often requiring manual synchronization), Rust enforces immutability for static variables by default. To enable mutation, synchronization primitives like RwLock or Mutex must be used, as seen with the lazy_static library in this tutorial. This design prioritizes thread safety and forces developers to explicitly handle concurrency, making Rust safer and less prone to hidden bugs in multithreaded environments.

#### Reflection Subscriber-2
> Have you explored things outside of the steps in the tutorial, for example: src/lib.rs? If not, explain why you did not do so. If yes, explain things that you have learned from those other parts of code.
Yes, exploring lib.rs provided valuable insights into the foundational components of the project. For example, the AppConfig struct and its generate method demonstrate how environment variables are loaded and merged using the dotenvy and Figment libraries, ensuring flexibility in configuration. The use of lazy_static to define REQWEST_CLIENT and APP_CONFIG as thread-safe, static variables highlights Rust's emphasis on safety and efficiency in managing shared resources. Additionally, the ErrorResponse struct and compose_error_response function illustrate a clean and reusable way to handle and format error responses in the Rocket framework. These components showcase how Rust's design principles, such as immutability, thread safety, and explicit error handling, are applied in practice to build robust and maintainable applications.

> Since you have completed the tutorial by now and have tried to test your notification system by spawning multiple instances of Receiver, explain how Observer pattern eases you to plug in more subscribers. How about spawning more than one instance of Main app, will it still be easy enough to add to the system?
Yes i have, the Observer pattern simplifies adding more subscribers because it decouples the publisher (Main app) from the subscribers (Receiver instances). Each Receiver instance can independently subscribe to notifications for specific product types without requiring changes to the publisher's code. This flexibility allows you to easily plug in additional subscribers by simply registering them with the publisher. Spawning multiple instances of the Main app is also straightforward, as each instance operates independently and communicates with the Receivers via HTTP requests. The pattern's loose coupling ensures scalability and maintainability, making it easy to expand the system with minimal effort.

> Have you tried to make your own Tests, or enhance documentation on your Postman collection? If you have tried those features, tell us whether it is useful for your work (it can be your tutorial work or your Group Project).
Yes, I have tried creating my own tests and enhancing the documentation in the Postman collection. Writing tests was particularly useful for verifying the correctness of the notification system and ensuring that all endpoints behaved as expected under different scenarios, such as valid and invalid inputs. Enhancing the Postman documentation helped streamline the testing process for both myself and my team by providing clear descriptions of each endpoint, required parameters, and expected responses. 
