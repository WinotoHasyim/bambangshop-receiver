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
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create SubscriberRequest model struct.`
    -   [x] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [x] Commit: `Implement add function in Notification repository.`
    -   [x] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [x] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 3: Implement services and controllers**
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [x] Commit: `Implement receive_notification function in Notification service.`
    -   [x] Commit: `Implement receive function in Notification controller.`
    -   [x] Commit: `Implement list_messages function in Notification service.`
    -   [x] Commit: `Implement list function in Notification controller.`
    -   [x] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1

1. **In this tutorial, we used RwLock<> to synchronise the use of Vec of Notifications. Explain why it is necessary for this case, and explain why we do not use Mutex<> instead?**

RwLock dipakai karena RwLock memperbolehkan beberapa thread untuk read data secara concurrent, tetapi hanya satu thread yang bisa write pada data dalam satu waktu. Mutex hanya memperbolehkan satu thread untuk read atau write pada data dalam satu waktu, sehingga RwLock lebih benefisial 

2. **In this tutorial, we used lazy_static external library to define Vec and DashMap as a “static” variable. Compared to Java where we can mutate the content of a static variable via a static function, why did not Rust allow us to do so?**

Di Rust, static variable adalah suatu shared data yang bisa diakses oleh semua thread. Shared data tersebut harus thread-safe untuk mencegah data race. Oleh karena itu, kita tidak bisa mengubah konten dari static variable secara langsung karena akan menghasilkan output yang unpredictable. Jika kita ingin mengubah konten pada static variable di Rust, kita bisa menggunakan RwLock untuk memastikan bahwa hanya satu thread yang bisa mengubah konten pada static variable dalam satu waktu.

#### Reflection Subscriber-2

1. **Have you explored things outside of the steps in the tutorial, for example: src/lib.rs? If not, explain why you did not do so. If yes, explain things that you have learned from those other parts of code.**

Sejauh ini, saya belum eksplor lebih dalam terkait hal-hal diluar tutorial seperti mengeksplor apa yang dilakukan oleh file lib.rs. Saya memilih untuk tidak mengeksplornya terlebih dahulu karena constraint waktu dan ada prioritas lain yang harus dilakukan yaitu mengerjakan Group Project.

2. **Since you have completed the tutorial by now and have tried to test your notification system by spawning multiple instances of Receiver, explain how Observer pattern eases you to plug in more subscribers. How about spawning more than one instance of Main app, will it still be easy enough to add to the system?**

Observer pattern memudahkan kita untuk menambah subscriber karena setiap subscriber hanya perlu untuk register ke main app untuk mulai memperoleh notifikasi. Teknik decoupling antara subject dengan observer ini memungkinkan kita untuk melakukan operasi pada suatu subscriber tanpa memengaruhi main app. Jika terdapat lebih dari satu instance dari main app, maka notification system ini akan menjadi lebih kompleks karena setiap main app instance mempunyai list of subscribersnya sendiri dan ini bisa memunculkan notifikasi yang duplikat pada suatu subscriber yang berada pada lebih dari satu main app instance. Untuk menghandle kasus ini, perlu dibuat implementasi sedemikian hingga setiap instance dari main app dapat berkomunikasi satu sama lain untuk memastikan bahwa pengiriman notifikasi hanya dilakukan sekali.

3. **Have you tried to make your own Tests, or enhance documentation on your Postman collection? If you have tried those features, tell us whether it is useful for your work (it can be your tutorial work or your Group Project).**

Saya belum mencoba membuat test atau meng-enhance dokumentasi pada postman collection, tetapi menurut saya, melakukan kedua hal tersebut dapat menjadi benefisial dalam pengerjaan group project nantinya. Dengan test, kita bisa memastikan bahwa kode kita bekerja dengan semestinya dan kita bisa mengidentifikasi bug dalam kode kita. Selain itu, meng-enhance Postman Documentation dapat membuat group saya lebih paham tentang API yang saya buat
