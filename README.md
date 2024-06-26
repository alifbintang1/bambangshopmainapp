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
1. Pada dasarnya, kebutuhan akan menggunakan interface atau trait dalam Observer pattern tergantung pada kompleksitas dari jenis observer yang akan digunakan. Dalam konteks BambangShop, jika kita hanya memiliki satu jenis observer, yaitu Subscriber, maka tidak perlu menggunakan trait atau interface. Namun, jika nantinya akan ada penambahan jenis observer baru, maka menggunakan trait akan menjadi lebih fleksibel.

2. Penggunaan DashMap lebih disarankan karena memungkinkan untuk memetakan setiap jenis produk ke setiap subscriber yang berlangganan. Dengan DashMap, kita dapat memiliki korelasi antara id program dan url subscriber dengan mudah. Menggunakan Vec akan memerlukan struktur data tambahan, seperti dua vector terpisah untuk menyimpan id program dan url, yang dapat menyulitkan dalam mengelola dan memperbarui data.

3. Dalam konteks keamanan multithreading Rust, DashMap merupakan pilihan yang tepat karena telah dioptimalkan untuk digunakan dalam lingkungan multithreaded. Penggunaan DashMap memastikan bahwa operasi pada map SUBSCRIBERS dapat dilakukan secara aman dan efisien oleh banyak thread. Meskipun Singleton pattern dapat memastikan hanya ada satu instance dari objek yang dibuat, namun hal itu tidak menjamin thread safety seperti yang disediakan oleh DashMap dalam kasus ini. Dengan menggunakan DashMap, kita dapat memastikan bahwa daftar subscriber terhadap produk kita dikelola secara thread-safe dan efisien.

#### Reflection Publisher-2
1. Memisahkan Service dari Repository dalam desain Model-View-Controller (MVC) diperlukan untuk mematuhi prinsip single responsibility. Service bertanggung jawab untuk mengelola logika bisnis dan pengolahan data yang diperoleh dari Repository, sementara Repository berfungsi sebagai antarmuka untuk mengakses dan memanipulasi data dalam database. Dengan memisahkan kedua lapisan ini, kode menjadi lebih terstruktur, mudah dipahami, dan lebih mudah dalam pemeliharaan.

2. Jika hanya menggunakan Model tanpa Service dan Repository, maka terjadi ketergantungan yang tinggi antar komponen, yang menyebabkan kesulitan dalam melakukan perubahan karena setiap perubahan pada Model akan berdampak pada seluruh kode. Hal ini meningkatkan kompleksitas dan menurunkan fleksibilitas dalam pengembangan dan pemeliharaan kode.

3. Postman adalah alat yang sangat berguna untuk menguji dan memvalidasi fungsionalitas dari aplikasi yang dikembangkan. Dengan Postman, saya dapat mengirimkan permintaan HTTP ke berbagai endpoint dalam aplikasi, serta memeriksa respon yang diterima untuk memastikan kebenaran dan konsistensi data. Saya juga dapat menggunakan fitur CRUD untuk menguji operasi dasar seperti membuat, membaca, memperbarui, dan menghapus data. Kemampuan untuk menyesuaikan permintaan dan melihat respons secara langsung membantu saya dalam menguji dan memperbaiki aplikasi dengan cepat dan efisien.

#### Reflection Publisher-3
1. Dalam kasus tutorial ini, kita menggunakan model push dari Observer Pattern. Hal ini terlihat dalam algoritma, di mana ketika terjadi perubahan pada objek, seperti pembuatan, penghapusan, atau pembaruan, layanan notifikasi akan memanggil metode yang akan memberi tahu semua subscriber tentang perubahan tersebut.

2. Jika kita menggunakan metode pull, setiap subscriber harus aktif memeriksa apakah terjadi perubahan data yang relevan untuk mereka. Keuntungannya adalah observer memiliki kendali penuh atas data yang mereka ambil dan kapan mereka mengambilnya. Namun, kerugiannya adalah observer perlu memiliki pengetahuan yang cukup tentang struktur data sumbernya untuk dapat melakukan tugas tersebut.

3. Jika kita memutuskan untuk tidak menggunakan multi-threading dalam proses notifikasi, maka akan terjadi penundaan saat NotificationService perlu memberi tahu setiap subscriber. Hal ini dapat mengakibatkan antrian panjang jika jumlah subscriber sangat banyak, dan menyebabkan keterbatasan dalam pengiriman notifikasi kepada setiap subscriber karena terhambat oleh keterbatasan komputasi yang ada.