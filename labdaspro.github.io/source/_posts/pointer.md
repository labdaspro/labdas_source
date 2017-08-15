---
title: Pointer
date: 2017-06-08 11:21:46
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

Pointer merupakan salah satu fitur unik bahasa C yang dapat digunakan untuk mengatur penggunaan memori secara *programatically*. Pointer juga merupakan salah satu alasan kenapa C menjadi bahasa pemrograman yang sangat powerful terutama untuk pengembangan perangkat lunak yang membutuhkan pengaturan memori secara *strict*.

Secara konseptual, semua variabel yang dideklarasikan akan memesan tempat pada memori, kemudian variabel pointer digunakan untuk menyimpan alamat memori variabel tersebut (dan alamat memori lain tentunya) untuk dimanipulasi lebih lanjut. Manipulasi memori dengan menggunakan pointer biasanya terbagi atas dua jenis yaitu:

1. Pengaksesan nilai yang tersimpan pada memori
2. Manipulasi Array Dinamis

### Pengaksesan Alamat Memori

Sebuah variabel yang dideklarasikan akan melakukan *booking* sebuah blok pada memori. Blok yang dipesan variabel tersebut memiliki sebuah alamat yang dan dapat diakses dengan menggunakan operator `&` (operator refference). Lebih jelasnya lihatlah contoh koding berikut:

```c
int main()
{
    int var1, var2;

    var1 = 50;
    var2 = 60;

    printf("%d\n", var1);
    printf("%d\n", var2);

    printf("%p\n", &var1);
    printf("%p\n", &var2);
}
```

Selanjutnya, setelah mengetahui alamat memori tersebut kita dapat menyimpannya ke dalam sebuah variabel pointer. Bukan hanya itu, kita bahkan bisa mengubah nilai yang tersimpan pada alamat tersebut melalui variabel pointer dengan menggunakan operator `*` (operator derefference), yang secara otomatis juga mengubah nilai yang tersimpan pada variabel. Lebih jelasnya, perhatikan contoh program berikut:

```c
int main()
{
    int var1, var2;
    int pointer1, pointer2;

    var1 = 50;
    var2 = 60;

    printf("%d\n", var1); // 50
    printf("%d\n", var2); // 60

    pointer1 = &var1;
    pointer2 = &var2;

    *pointer1 = 100;
    *pointer2 = 200;

    printf("%d\n", var1); // 100
    printf("%d\n", var2); // 200
}
```

Baris 12 dan 13 pada koding di atas kita melakukan pengisian nilai variabel `pointer` dan `pointer1` dengan alamat memori dari `var1` dan `var2`, selanjutnya pada baris 15 da 16 kita menggunakan operator `*` (derefference) untuk mengisikan nilai pada alamat memori yang tersimpan pada kedua variabel pointer tersebut. Secara mudahnyanya `*pointer1 = 100;` dapat diterjemahkan sebagai "isikan nilai 100 pada variabel yang alamat memorinya tersimpan pada pointer1".

Beberapa dari kita mungkin akan berfikir "kenapa harus repot-repot mengubah nilai variabel melalui pointer, jika kita bisa merubahnya secara langsung?". Masalahnya adalah, ketika kita membuat program dengan bahasa C, terkadang ada beberapa kasus kita tidak bisa merubah nilai variabel secara langsung, sehingga kita perlu menggunakan pointer untuk melakukannya. Contoh dari permasalahan tersebut adalah fungsi `swap` sebagaimana berikut:

```c
void swap(int *pnum1, int *pnum2)
{
    int temp;
    temp = *pnum1;
    *pnum1 = *pnum2;
    *pnum2 = temp;
}

int main()
{
    int var1, var2;
    var1 = 20;
    var2 = 50;

    printf("%d\n", var1); // 20
    printf("%d\n", var2); // 50

    swap(&var1, &var2);

    printf("%d\n", var1); // 50
    printf("%d\n", var2); // 20
}
```

Ketika membuat fungsi `swap` kita tidak tahu nama variabel yang akan ditukar nilainya, sehingga kita menyediakan parameter pointer sebagai tempat penampung alamat memori dari variabel yang akan ditukar nilainya (baris 1). Pada baris 4 hingga 6 terlihat pengaksesan nilai yang tersimpan pada variabel pointer dilakukan dengan menggunakan operator `*` (derefference), baik untuk pengaksesan baca maupun tulis. Terakhir, ketika fungsi `swap` di panggil pada baris 18, menyebabkan nilai pada variabel `var1` dan `var2` tertukar.

### Manipulasi Array Dinamis

Kegunaan pointer yang kedua adalah untuk pembuatan atau pengaksesan *array* dinamis. Selama ini untuk dapat menggunakan *array* kita mendeklarasikannya secara statis dengan menggunakan ukuran pasti. Artinya setelah deklarasi *array*, kita tidak dapat mengubah ukuran dari *array* tersebut. Nah dengan menggunakan pointer dan fungsi `malloc` kita dapat mendeklarasikan sebuah *array* dengan ukuran tertentu dan ukurannya dapat diubah setelah deklarasi. Kode berikut ini menampilkan contoh pembuatan *array* dinamis dengan fungsi `malloc`:

```c
int main()
{
    int *darray;
    darray = malloc(10 * sizeof darray);
    for(int i = 0; i < 10; i++)
    {
        darray[i] = i+1;
    }

    darray = malloc(20 * sizeof darray);
    for(int i = 0; i < 20; i++)
    {
        darray[i] = i+1;
    }
}
```

Pada cuplikan kode di atas, kita melakukan inisalisasi pada variabel pointer `darray` dengan mengalokasikan 10 blok memori integer dengan menggunakan perintah `malloc`. Bisa dibilang baris ke 3 dan 4 sama artinya dengan deklarasi variabel `int darray[10];`. Hanya saja, karena menggunakan pointer untuk *array* dinamis, kita dapat mengubah ukuran *array* dengan menginisialisasi kembali variabel tersebut, sebagaimana ditunjukkan pada baris 10.

#### Pointer sebagai Parameter *Array*

Penggunaan pointer untuk *array* dinamis yang lain dapat kita temui ketika kita ingin membuat fungsi yang menerima *array* sebagai paramternya.