# catatan-penting-dart

### Higher-Order Functions
Setelah mempelajari modul sebelumnya, Anda mungkin bertanya apa yang bisa
dilakukan dengan lambda atau anonymous function?
Kita bisa memanfaatkan lambda untuk membuat higher-order function. Higher order
function adalah fungsi yang menggunakan fungsi lainnya sebagai parameter, menjadi
tipe kembalian, atau keduanya. Coba perhatikan fungsi berikut:
```dart
. void myHigherOrderFunction(String message, Function myFunction) {
. print(message); . print(myFunction(3, 4)); . }
```
Fungsi di atas merupakan higher order function karena menerima parameter berupa
fungsi lain. Untuk memanggil fungsi di atas, kita bisa langsung
memasukkan lambda sebagai parameter maupun variabel yang berisi nilai berupa
```dart
fungsi. . // Opsi 1
. Function sum = (int num1, int num2) => num1 + num2; . myHigherOrderFunction('Hello', sum); .
.
. // Opsi 2
. myHigherOrderFunction('Hello', (num1, num2) => num1 + num2);
Jika disimulasikan fungsi myHigherOrderFunction akan memanggil fungsi sum yang
dijadikan parameter. . void myHigherOrderFunction(String message, Function myFunction) {
. print(message); . print(myFunction(3, 4)); // sum(3, 4) // return 3 + 4
. }
```
Namun deklarasi higher order function ini bisa menjadi sedikit tricky. Misalnya kode
di bawah ini tidak akan terdeteksi eror namun ketika dijalankan, aplikasi Anda akan
mengalami crash. Tahukah kenapa?
```dart
. void myHigherOrderFunction(String message, Function myFunction) {
. print(message); . print(myFunction(4)); . }
```
Karena kita tidak menentukan spesifikasi dari fungsi seperti jumlah parameter atau
nilai kembaliannya, maka semua jenis fungsi akan bisa dijalankan termasuk
pemanggilan myFunction seperti di atas. Untuk mengatasinya kita bisa lebih spesifik
menentukan seperti apa fungsi yang valid untuk menjadi parameter.
```dart
. void myHigherOrderFunction(String message, int Function(int num1, int
num2) myFunction) { }
```
Pada fungsi di atas kita perlu memasukkan fungsi dengan dua parameter dan nilai
kembali berupa int sebagai parameter. Pada materi collection sebenarnya kita telah menggunakan satu fungsi yang
merupakan higher order function yaitu fungsi forEach(). Sebagai contoh kita punya
daftar bilangan fibonacci yang disimpan ke sebuah variabel. . 
```dart
var fibonacci = [0, 1, 1, 2, 3, 5, 8, 13];
```
IntelliJ IDEA akan menunjukkan suggestion apa saja yang perlu menjadi parameter. Kita bisa melihat bahwa forEach membutuhkan satu parameter berupa fungsi.
Sehingga ketika memanggil fungsi ini kita bisa melakukan operasi pada
masing-masing item misalnya mencetak ke konsol. 
```dart
. fibonacci.forEach((item) {
. print(item); . });
```
### Closures
Suatu fungsi dapat dibuat dalam lingkup global atau di dalam fungsi lain. Suatu fungsi
yang dapat mengakses variabel di dalam lexical scope-nya disebut
dengan closure. Lexical scope berarti bahwa pada sebuah fungsi bersarang (nested
functions), fungsi yang berada di dalam memiliki akses ke variabel di lingkup
induknya. Berikut ini adalah contoh kode implementasi closure: 
```dart
. void main() {
. var closureExample = calculate(2); . closureExample(); . closureExample(); . }
.
. Function calculate(base) {
. var count = 1; .
. return () => print("Value is ${base + count++}"); . }
```
Ketika kode di atas dijalankan, konsol akan tampil seperti berikut: 
. Value is 3
. Value is 4
Di dalam fungsi calculate() terdapat variabel count dan mengembalikan nilai berupa
fungsi. Fungsi lambda di dalamnya memiliki akses ke variabel count karena berada
pada lingkup yang sama. Karena variabel count berada pada scope calculate, maka
umumnya variabel tersebut akan hilang atau dihapus ketika fungsinya selesai
dijalankan. Namun pada kasus di atas fungsi lambda atau closureExample masih
memiliki referensi atau akses ke variabel count sehingga bisa diubah. Variabel pada
mekanisme di atas telah tertutup (close covered), yang berarti variabel tersebut berada
di dalam closure.
### Dart Type System
Dalam bahasa pemrograman, type system adalah sistem logis yang terdiri dari
seperangkat aturan yang menetapkan properti atau tipe ke berbagai konstruksi
program komputer, seperti variabel, expression, fungsi, atau modul. Type system ini
memformalkan atau memberikan standar kategori tersirat yang
digunakan programmer untuk tipe data, struktur data, atau komponen lainnya. Dart menyebut type system-nya sebagai sound type system. Soundness ini berarti
program Anda tidak akan pernah bisa memasuki keadaan di mana sebuah ekspresi
mengevaluasi nilai yang tidak cocok dengan jenis tipenya. Sound type system pada Dart ini sama dengan type system pada Java atau C#. Di
mana kondisi soundness ini dicapai dengan menggunakan kombinasi pemeriksaan
statis (compile-time error) dan pemeriksaan saat runtime. Sebagai contoh, menetapkan String ke variabel int adalah kesalahan compile-time. Casting Object ke String dengan as String akan gagal ketika runtime jika objek
tersebut bukan String. Manfaat dari sound type system ini, antara lain:
Mengungkap bug terkait tipe pada saat compile time. Sound type system memaksa kode untuk tidak ambigu tentang tipenya,
sehingga bug terkait tipe yang mungkin sulit ditemukan saat runtime, bisa
ditemukan pada waktu kompilasi. Kode lebih mudah dibaca. Kode menjadi lebih mudah dibaca karena Anda dapat mengandalkan nilai
yang benar-benar memiliki tipe yang ditentukan. Tipe pada Dart tidak bisa
berbohong. Kode lebih mudah dikelola. Ketika Anda mengubah satu bagian kode, type system dapat memperingatkan
Anda tentang bagian kode mana yang baru saja rusak. Kompilasi ahead of time (AOT) yang lebih baik. Kode yang dihasilkan saat kompilasi AOT menjadi jauh lebih efisien. Generic
Jika Anda perhatikan pada dokumentasi collection seperti List, sebenarnya tipe
dari List tersebut adalah List<E>. Tanda <...> ini menunjukkan bahwa List adalah
tipe generic, tipe yang memiliki tipe parameter. Menurut coding convention dari Dart, tipe parameter dilambangkan dengan satu huruf kapital seperti E, T, K, atau V. Secara umum generic merupakan konsep yang digunakan untuk menentukan tipe data
yang akan kita gunakan. Kita bisa mengganti tipe parameter generic pada Dart dengan
tipe yang lebih spesifik dengan menentukan instance dari tipe tersebut.
Sebagai contoh, perhatikan List yang menyimpan beberapa nilai berikut: 
```dart
. List<int> numberList = [1, 2, 3, 4, 5];
 ```
Tipe parameter yang digunakan pada variabel list di atas adalah int, maka nilai yang
bisa kita masukkan adalah nilai dengan tipe int. Begitu juga jika kita menentukan tipe
parameter String, maka tipe yang bisa kita masukkan ke dalam list hanya
berupa String. 
  ```dart
 . List<int> numberList = [1, 2, 3, 4, 5]; 
 . List<String> stringList = ['Dart', 'Flutter', 'Android', 'iOS']; 
 . List dynamicList = [1, 2, 3, 'empat']; // List<dynamic>
  ```
Berbeda jika kita tidak menentukan tipe parameter dari list. List tersebut tidak
memiliki tipe yang menjadi acuan bagi kompiler sehingga semua tipe bisa disimpan
ke dalam list. Variabel dynamicList di atas sebenarnya masih menerapkan generic
dengan tipe dynamic sehingga tipenya menjadi List<dynamic>. Dari kasus di atas kita bisa simpulkan bahwa Dart membantu kita menghasilkan kode
yang type safe dengan membatasi tipe yang bisa digunakan ke dalam suatu objek dan
menghindari bug. Selain itu generic juga bermanfaat mengurangi duplikasi kode. Misalnya ketika Anda perlu untuk menyimpan objek cache bertipe String dan int. Alih-alih membuat dua objek StringCache dan IntCache, Anda bisa membuat satu
objek saja dengan memanfaatkan tipe parameter dari generic. . abstract class Cache<T> {
. T getByKey(String key); . void setByKey(String key, T value); . }
Dengan Dart type system kita bisa mengganti tipe parameter yang digunakan sesuai
dengan susunan hierarkinya. Perhatikan hierarki objek Animal berikut:
Dengan hierarki di atas, jika kita memiliki objek List<Bird> maka objek apa saja
yang bisa kita masukkan ke list tersebut?
. List<Bird> birdList = [Bird(), Dove(), Duck()];
Seluruh objek Bird atau objek turunannya bisa masuk ke dalam birdList. Namun, ketika menambahkan objek dari Animal, terjadi compile error karena
objek Animal belum tentu merupakan objek Bird.
  ```dart
  . List<Bird> birdList = [Bird(), Dove(), Duck(), Animal()]; // Error
  ```
Berbeda jika kita mengisi List<Bird> dengan List<Animal> seperti berikut:
  ```dart
  . List<Bird> myBird = List<Animal>();
  ```
Kompiler tidak akan menunjukkan eror namun ketika kode dijalankan akan
terjadi runtime error karena List<Animal> bukanlah subtype dari
  ```dart
  List<BIrd>. . Unhandled exception: 
  . type 'List<Animal>' is not a subtype of type 'List<Bird>'
  ```
  ### Type Inference
Seperti yang kita tahu Dart mendukung type inference. Dart memiliki analyzer yang
dapat menentukan menyimpulkan tipe untuk field, method, variabel lokal, dan
beberapa tipe argumen generic. Ketika analyzer tidak memiliki informasi yang cukup
untuk menyimpulkan tipe tertentu, maka tipe dynamic akan digunakan. Misalnya berikut ini adalah contoh penulisan variabel map dengan tipe yang eksplisit: 
 ```dart
 . Map<String, dynamic> company = {'name': 'Dicoding', 'yearsFounded': 2
015};
 ```
Atau, Anda dapat menggunakan var dan Dart akan menentukan tipenya. 
```dart 
var company = {'name': 'Dicoding', 'yearsFounded': 2015}; // Map<String, Object>
 ```
Type inference menentukan tipe dari entri kemudian menentukan tipe dari variabelnya. Pada contoh di atas, kedua key dari map adalah String, namun nilainya memiliki tipe
yang berbeda, yaitu String dan int, di mana keduanya merupakan turunan dari Object. Sehingga variabel company akan memiliki tipe Map<String, Object>.
Saat menetapkan nilai objek ke dalam objek lain, kita bisa mengganti tipenya dengan
tipe yang berbeda tergantung pada apakah objek tersebut
adalah consumer atau producer. Perhatikan assignment berikut:
  ```dart
  . Fish fish = Fish();
  ```
Fish fish adalah consumer dan Fish() adalah producer. Pada posisi consumer, aman
untuk mengganti consumer bertipe yang spesifik dengan tipe yang lebih umum. Jadi, aman untuk mengganti Fish fish dengan Animal
fish karena Animal adalah supertype dari Fish. 
  ```dart
  . Animal fish = Fish();
  ```
Namun mengganti Fish fish dengan Shark fish melanggar type safety karena bisa
saja Fish memiliki subtype lain dengan perilaku berbeda, misalnya FlyingFish. 
  ```dart
  . Shark fish = Fish(); // Error
  ```
Pada posisi producer, aman untuk mengganti tipe yang umum (supertype) dengan tipe
yang lebih spesifik (subtype). . Fish fish = Shark();
