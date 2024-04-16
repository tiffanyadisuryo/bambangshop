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
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [ ] Commit: `Create Subscriber model struct.`
    -   [ ] Commit: `Create Notification model struct.`
    -   [ ] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [ ] Commit: `Implement add function in Subscriber repository.`
    -   [ ] Commit: `Implement list_all function in Subscriber repository.`
    -   [ ] Commit: `Implement delete function in Subscriber repository.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
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
1. Observer design patterns didefinisikan saat terdapat observer yang menerima notification setiap ada perubahan state dengan pemanggilan method-nya. Untuk menjawab apakah dibutuhkan interface atau trait tergantung pada requirements. Jika subscribers memiliki sifat yang berbeda, maka mendefinisikan trait akan berguna karena menyediakan implementasi yang banyak dari subscriber interface.

2. DashMap diperlukan karena pada dasarnya DashMap dapat memberi syarat id atau url unik. Selain itu, dapat memeriksa adanya key atau url pada DashMap dengan mudah sehingga memastikan tidak ada yang duplikat.

3. Singleton pattern digunakan untuk memastikan sebuah class hanya memiliki satu instance dan memberikan global point access pada instance tersebut. Maka jika ingin mengaplikasikannya, harus menambahkan mekanisme sinkronisasi agar concurrent access pada SUBSCRIBERS dapat tetap dilakukan dengan aman. Sedangkan, DashMap didesain untuk menyediakan thread-safe concurrent access, sehingga lebih cocok menggunakan DashMap.

#### Reflection Publisher-2
1. Service perlu dipisah agar dapat memenuhi sifat single responsibility principle, mempromosikan modularitas, memudahkan testing, mendukung fleksibilitas dan skalabilitas, mengkapsulasi logika akses data, dan meningkatkan kejelasan dan pemeliharaan kode secara keseluruhan.

2. Jika hanya menggunakan Model tanpa melakukan separasi menjadi service maupun repository, maka kompleksitas kode nya akan naik, yang menyebabkan penurunan maintainability, dan hubungan antar model akan sangat terikat sehingga bila ada perubahan di salah satu model, maka akan memengaruhi model lainnya dan butuh diubah juga.

3. Menurut saya, Postman sangat berguna dan mempermudah dalam melihat kebenaran kode kita. Saat terdapat sebuah request dapat dilihat response yang dikembalikan dan data yang diambil benar atau salah.

#### Reflection Publisher-3
1. Variasi yang digunakan adalah push model, ini dikarenakan pada function create, delete, dan publish, subscriber dari product_type menerima notifikasi.

2. Jika menggunakan variasi pull, observer lebih memiliki kebebasan dan keamanan terhadap data tanpa andil publisher. Namun, butuh banyak effort dan pemahaman untuk melakukan hal tersebut.

3. Tanpa multi-threading, maka notifikasi akan dikirim bergantian secara sinkronus tentunya jadi sangat lambat.