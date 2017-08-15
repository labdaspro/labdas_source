---
title: Fungsi
date: 2017-06-02 15:13:27
tags: fungsi
---

Fungsi merupakan suatu bagian program yang dimaksudkan untuk mengerjakan tugas tertentu dan letaknya terpisah dari program yang memanggilnya.

Dalam setiap program dalam bahasa C, minimal terdapat satu buah fungsi yaitu fungsi `main()`. Berikut adalah contoh pembuatan fungsi dalam bahasa C.

Kentungan dalam penggunaan fungsi meliputi:

- Program akan memiliki struktur yang jelas (mempunyai readability yang tinggi)
- Bersifat reusability (dapat digunakan kembali) sehingga akan menghindari penulisan program yang sama.

Dalam bahasa C, fungsi dapat diklasifikasikan menjadi dua yaitu:

1. Fungsi pustaka atau fungsi yang telah tersedia dalam Bahasa C.
2. Fungsi yang didefinisikan atau dibuat oleh programmer.

Cara membuat fungsi sendiri dalam bahasa C adalah sebagai berikut:

Sebelum digunakan (dipanggil), suatu fungsi harus dideklarasikan dan didefinisikan terlebih dahulu. 
Bentuk umum pendeklarasian fungsi adalah :

```
tipe_fungsi nama_fungsi(parameter_fungsi);
```

Sedangkan bentuk umum pendefinisian fungsi adalah :

```
tipe_fungsi nama_fungsi(parameter_fungsi)
{ 
    statement
    statement
    ………...
    ………...
}
```



```c
void print_array(int * data, int n)
{
    for(int i = 0; i < n; i++)
        printf("%d", data[i]);
}
```