---
title: Tipe Data Bentukan
date: 2017-06-07 14:09:47
tags:
    - struct
    - class
    - abstract data type
    - tipe data
categories:
    - bahasa C
    - tipe data
    - struct
---

Dalam perjalanan pembuatan program, ada kalanya kita memerlukan tipe data baru yang tidak terdefinisikan sebelumnya pada sebuah bahasa pemrograman. Misalkan untuk menyimpan nilai waktu kita memerlukan tiga variabel yang saling terhubung, yaitu tiga variabel dengan tipe data integer untuk meyimpan nilai jam, menit, dan detik. Jika menggunakan cara konvensional yaitu mendeklarasikan tiga variabel secara manual kemudian mengisikan nilainya, maka akan cukup merepotkan. Belum lagi jika data waktu yang perlu kita simpan dan manipulasi lebih dari satu dan perlu disimpan dengan menggunakan *array*.

<!--more-->
## Deklarasi Struktur

Sebagai solusi dari permasalahan tersebut, bahasa pemrograman biasanya menyediakan mekanisme khusus untuk membentuk tipe data baru dengan menggabungkan tipe data primitif yang sudah ada. Dalam bahasa pemrograman C, perintah yang digunakan untuk membentuk struktur tipe data baru adalah mengguanakan perintah `struct` yang diikuti nama tipe data dan komponen-komponen tipe datanya. Berikut adalah contoh pembuatan/deklarasi struktur tipe data **time** dengan menggunakan perintah `struct`:

```c
struct time{
    int jam;
    int menit;
    int detik;
}

struct time var_time;
```

Pada contoh koding di atas, terlihat pada baris 1 - 4 cara deklarasi tipe data **time** dengan menggunakan perintah `struct` yang diikuti dengan nama struktur dan komponen tipe datanya. Sedangkan pada baris 6 ditunjukkan cara penggunaan struktur baru yang telah dideklarasikan adalah menggunakan perintah `struct` yang diikuti nama struktur dan nama variabelnya.

Setelah mendeklarasikan struktur serta variabelnya, kita dapat mengakses komponen dari tipe data tersebut dengan menggunakan operator `.` (titik) setelah nama variabelnya. Contoh pengaksesan komponen pada struktur sebagaimana ditunjukkan pada koding berikut:

```c
struct time var_time;
var_time.jam = 2;
var_time.menit = 40;
var_time.detik = 33;

printf("%d:%d:%d", var_time.jam, var_time.menit, var_time.detik);
```

Koding pada baris 2 - 4 mengakses komponen jam, menit, dan detik untuk mengisikan nilai pada komponen variabel tersebut, sedangkan pada baris 6 nilai yang telah diisikan diakses untuk dicetak menggunakan perintah `printf`.

Deklarasi variabel struct baru harus selalu menggunakan kata kunci `struct` untuk menunjukkan bahwa tipe data `time` adalah tipe data dalam bentuk struktur. Namun kita dapat mengguanakan perintah `typedef` untuk memberikan alias pada struktur tersebut sehingga kita dapat menggunakannya selayaknya tipe data primitif lain. Contoh penggunaan typdef ditunjukkan pada koding di bawah ini:

```c
struct s_time{
    int jam;
    int menit;
    int detik;
} time;

time new_time;
```

## Fungsi Operator

Membuat struktur tipe data baru saja tidak cukup. Agar tipe data yang telah kita buat dapat digunakan dengan baik, selain mendeklarasikan strukturnya, kita juga harus membuatkan fungsi-fungsi untuk mengoperasikan tipe data tersebut. Berdasarkan kategori penggunaannya, ada empat jenis fungsi operator yang biasanya dibuat setiap membuat tipe data baru yaitu:

- **Fungsi konstruktor**: fungsi yang digunakan untuk membentuk tipe data baru
- **Fungsi selector**: fungsi yang digunakan untuk mendapatkan nilai komponen atau nilai tertentu dari tipe data baru
- **Fungsi validator**: fungsi yang digunakan untuk memastikan bahwa nilai yang diinputkan pada komponen tipe data valid sesuai logika yang dibuat.
- **Fungsi manipulasi**: fungsi untuk melakukan berbagai macam memanipulasi tipe data yang telah dibuat.

Berikut adalah dua contoh fungsi konstruktor yang digunakan untuk membuat tipe data **time** dengan menggunakan dua alternatif cara:

```c
time make_time(int j, int m, int d)
{
  time waktu;
  waktu.j = j;
  waktu.m = m;
  waktu.d = d;

  return waktu;
}

time seconds_2_time(int seconds)
{
    time waktu;
    waktu.jam = seconds / 3600;
    waktu.menit = (seconds % 3600) / 60;
    waktu.detik = seconds % 60;

    return time;
}

int main()
{
    time new_time1 = make_time(5, 30, 25);
    time new_time2 = seconds_2_time(3755);
    return 0;
}
```

Fungsi `make_time` adalah contoh fungsi konstruktor paling sederhana, di mana fungsi tersebut hanya menerima inputan untuk komponen struktur melalui parameter dengan jumlah yang sama persis dengan komponen struktur dan tidak melakukan validasi apapun. Meskipun fungsi tersebut sangat sederhana, tapi dengan menggunakan fungsi konstruktor tersebut minimal kita dapat menghemat waktu inisialisasi atau pengisian nilai pada tipe data waktu jika dibanding dengan cara manual mengisi komponen satu persatu. Sedangkan fungsi `seconds_2_time` juga adalah konstruktor untuk tipe data **time**. Fungsi ini menggunakan pendekatan yang berbeda yaitu dengan mengonversi nilai detik yang diberikan pada parameter dan merubahnya ke dalam tipe data waktu.

Selanjutnya untuk contoh fungsi selektor diberikan pada koding di bawah ini:

```c
int get_hour(time waktu)
{
    return waktu.jam;
}

int get_second(time waktu)
{
    return waktu.jam;
}

int time_2_second(time waktu)
{
    return ((waktu.jam * 3600) + (waktu.menit * 60) + waktu.detik);
}
```

Pada contoh yang diberikan di atas fungsi `get_hour` dan `get_second` merupakan contoh fungsi sederhana dari selector. Yang dilakukan kedua fungsi tersebut hanya mengembalikan nilai jam dan detik dalam satuan *integer*. Adapun fungsi `time_2_second` merupakan contoh pengembalian nilai tertentu dari tipe data waktu dalam bentuk khusus, yang dalam kasus ini mengembalikan jumlah detik dari satuan waktu yang diberikan pada parameter.

Fungsi validator bertujuan untuk memastikan satuan waktu yang diinputkan sudah valid sesuai dengan logika yang diinginkan. Berikut ini adalah contoh fungsi validator beserta implementasinya dalam konstruktor:

```c
int valid_time(time waktu)
{
    return (((waktu.jam >= 0) && (waktu.jam < 24)) && 
            ((waktu.menit >= 0) && (waktu.menit < 60)) &&
            ((waktu.detik >= 0) && (waktu.detik) < 60));
}

// pengembangan fungsi konstruktor dengan validasi
time make_time(int jam, int menit, int detik)
{
  time waktu;
  waktu.j = j;
  waktu.m = m;
  waktu.d = d;

  if(valid_time(waktu))
    return waktu;
  else
    return make_time(0, 0, 0);
}

int main()
{
    time waktu1 = make_time(5, 23, 50); // -> 5:23:50
    time waktu2 = make_time(20, 87, -9); // -> 0:0:0
    return 0;
}
```

Terlihat pada koding di atas, fungsi `valid_time` melakukan validasi terhadap nilai waktu yang diberikan pada parameter dan mengembalikan nilai *true* atau *false*. Selanjutnya pada fungsi konstruktor `make_time` fungsi tersebut digunakan untuk mengecek apakah waktu yang diinputkan sudah valid atau tidak, jika tidak maka fungsi konstruktor akan mengembalikan waktu *default* yaitu **0:0:0**.

Kategori terakhir adalah fungsi manipulasi yang mengerjakan tugas selain ketiga kategorisasi sebelumnya. Berikut di bawah ini adalah beberapa contoh fungsi manipulasi waktu:

```c
void print_time(time waktu)
{
    printf("%d:%d:%d", waktu.jam, waktu.menit, waktu.detik);
}

time add_time(time waktu1, time waktu2)
{
    return second_2_time(time_2_second(waktu1) + time_2_second(waktu2));
}

void print_times(time * time_data, int n)
{
    for(int i = 0; i < n; i++)
    {
        print_time(time_data[i]);
        printf("\n");
    }
}
```

Ketiga fungsi di atas adalah contoh manipulasi tipe data **time** yang tidak berhubungan dengan konstruktor, selector, atau validator. Dua diantaranya `add_time` dan `print_times` memanfaatkan fungsi-fungsi operator yang pernah dibuat sebelumnya.

## Array dan Pointer of Struct

Seperti halnya tipe data lain, tipe data yang telah dibuat dapat digunakan untuk array dan pointer. Deklarasi maupun penggunaan array of struct tidak jauh berbeda dengan penggunaan tipe data biasa, begitu juga untuk pointer untuk keperluan dynamic array juga tidak jauh berbeda sebagaimana ditunjukkan pada contoh koding di bawah ini:

```c
time data_waktu[10];
data_waktu[0] = make_time(5, 12, 18);
data_waktu[1] = make_time(19, 20, 00);

print_time(data_waktu[0]);
printf("Jam kedua adalah %d\n", data_waktu[1].jam);

// deklarasi waktu untuk pointer
time * p_waktu;
// deklarasi array dinamis dengan malloc
p_waktu = malloc(10 * sizeof p_waktu);
p_waktu[0] = make_time(20, 15, 00);
printf("Menit dari waktu adalah %d\n", p_waktu[0].menit);
```

Contoh koding di atas menunjukkan bahwa deklarasi dan penggunaan array dan pointer of struct memang tidak terlalu berbeda dengan tipe data primitif, baik untuk penggunaan array statis maupun array dinamis dengan menggunakan perintah `malloc`.

Meski demikian untuk pointer of struct yang digunakan untuk mengakses alamat memori dari variabel lain (*indirect access*) ada pengecualian pada cara pengaksesan elemennya yaitu menggunakan operator `->` (panah) bukan `.` (titik). Contoh pengaksesan tersebut dapat dilihat pada koding di bawah ini.

```c
time waktu = make_time(5, 12, 15);
time * p_waktu = &waktu;

print_time(waktu); // --> 5:12:15

p_waktu->jam = 20;
p_waktu->menit = 10;
p_waktu->detik = 0;

print_time(waktu); // --> 20:10:0

*p_waktu = make_time(10, 20, 30);

print_time(waktu); // --> 10:20:30
```

## Struct Sebagai Elemen Struct Lain

Setelah mendeklarasikan sebuah struktur, maka struktur tersebut dapat digunakan sebagaimana tipe data lain, termasuk diantaranya adalah digunakan untuk membuat struktur baru. Di bawah ini diberikan contoh struct `time` yang sudah dideklarasikan sebelumnya digunakan pada struct `schedule` untuk membentuk tipe data baru lagi:

```c
typedef struct schedule
{
    time start, finish;
    char *event;
}

schedule make_schedule(time start, time finish, char *event)
{
    schedule new_schedule;
    new_schedule.start = start;
    new_schedule.finish = finish;
    strcpy(new_schedule.event, event);

    return new_schedule;
}

int schedule_cmp(schedule s1, schedule s2)
{
    if(time_2_second(s1.start) > (time_2_second(s2.start)))
        return 1;
    else if(time_2_second(s1.start) < (time_2_second(s2.start)))
        return -1;
    else
        return 0;
}

int main()
{
    schedule event1, event2;
    event1 = make_schedule(make_time(8, 0, 0), make_time(8, 30, 0),
                               "having breakfast");
    event2 = make_schedule(make_time(6, 30, 0), make_time(7, 30, 0),
                               "take a bath");
    schedule_cmp(event1, event2); // --> -1
    return 0;
}
```

Koding di atas menunjukkan pembuatan tipe data baru dengan memanfaatkan tipe data baru **time** yang telah kita buat sebelumnya. Baris 1 - 5 adalah deklarasi struktur **schedule** yang memiliki dua elemen dengan tipe data **time**. Selanjutnya terdapat dua fungsi operator untuk tipe data **schedule**  yaitu `make_schedule` yang merupakan fungsi konstruktor dan `schedule_cmp` yang dapat digunakan untuk membandingkan dua tipe data **schedule**. Pada fungsi `main` yang merupakan bagian pemanggilan fungsi `make_schedule` terlihat bahwa kita juga bisa menggunakan fungsi `make_time` untuk memudahkan pengisian nilai parameter pertama dan kedua.