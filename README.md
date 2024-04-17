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
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Subscriber model struct.`
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [x] Commit: `Implement add function in Subscriber repository.`
    -   [x] Commit: `Implement list_all function in Subscriber repository.`
    -   [x] Commit: `Implement delete function in Subscriber repository.`
    -   [x] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [x] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [ ] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [ ] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [ ] Commit: `Implement publish function in Program service and Program controller.`
    -   [ ] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?

Dalam buku Head First Design Pattern, Subscriber memang didefinisikan sebagai interface.  Namun, pada kasus BambangShop, kita bisa saja tidak menggunakan interface (atau trait di Rust) dengan menganggap bahwa semua Subscriber memiliki perilaku yang sama. Sehingga, mplementasi trait dapat dianggap tidak perlu dan cukup menggunakan satu Model struct. 

2. id in Product and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?

Sebenarnya pengguna Vec (list) sudah cukup dalam kasus ini karena masih bisa menjamin keunikan id. Akan tetapi, untuk mengutamakan skalabilitas, DashMap lebih cocok karena menawarkan performa yang lebih baik dibanding Vec (list). DashMap menawarkan thread-safety dan kemudahan akses data dengan key unik.

3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?


Penggunaan DashMap untuk thread-safety pada kaus ini sudah tepat. Hal ini dikarenakan DashMap dari library eksternal dibutuhkan untuk membuat HashMap thread-safe di Rust. Thread-safety diperlukan untuk mencegah adanyarace condition antara suatu thread dengan thread lain. Sementara, Singleton Pattern kurang cocok. Singleton pattern tidak menjamin thread-safety secara inheren. Singleton justeri seringkali membutuhkan modifikasi tambahan untuk memastikan thread-safety.

#### Reflection Publisher-2
1. In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?

Berdasarkan prinsip SOLID, salah satunya adalah Single Responsibility Principle (SRP), prinsip ini mendorong pemisahan antara Service dan Repository. Service menangani logika bisnis yang kompleks, dan Repository mengurusi penyimpanan dan pengambilan data. Pemisahan ini membuat kode lebih terstruktur dan mudah dipelihara. Pemisahan ini juga menunjukan adanya tanggung jawab yang berbeda dari tiap komponen sehingga kita dapat memisahkannya dengan lebih jelas. Pengelolaan dan pengembangannya pun dapat sesuai dengan tiap komponen yang ingin dikembangkan, bukan secara keseluruhan.

2. What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?

Jikia kita hanya menggunakan Model dalam kerangka MVC, dan tidak melakukan pemisahan antara Service dan Repository, tanggung jawab model akan menjadi terlalu besar. Akibatnya, model bisa menjadi terlalu kompleks dan sulit untuk dipahami dan dipelihara. Pengetesan juga akan menjadi lebih sulit karena logika bisnis yang ada dalam model tercampur dengan penyimpanan data yang ada.

3. Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects

Postman adalah tool yang dapat digunakan untuk melakukan pengujian API. Postman dapat membuat dan mengirim Request HTTP untuk menguji berbagai jenis fungsi program. Postman juga dapat menganalisis repon yang diberikan server secara detail yang dapat membantu kita untuk memverifikasi fungsionalitas program. Salah satu fitur unik yang terdapat di Postman adalah Environments yang dapat mengatur Postman untuk melakukan pengujian di berbagai environments yang ingin kita ujikan.

#### Reflection Publisher-3
