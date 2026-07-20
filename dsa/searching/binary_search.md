# Binary Search

## Memahami Binary Search dengan Analogi Buku Telepon

Bayangkan kita ingin mencari nama Sam di sebuah buku telepon.

Jika kita mulai dari halaman pertama (huruf A) lalu membalik halaman satu per satu, pencarian akan memakan waktu lama.

Namun, buku telepon memiliki satu kelebihan: semua nama sudah diurutkan berdasarkan alfabet. Karena itu, kita bisa menggunakan cara yang lebih cerdas.

Daripada mulai dari awal, kita buka halaman di tengah.

```
[A ... K ... R ... Z]
         ^
      mulai di tengah
```

Misalnya halaman tengah berisi nama-nama yang diawali huruf K.

Karena kita mencari Sam, dan huruf S berada setelah K, maka kita langsung tahu bahwa Sam tidak mungkin berada pada halaman A sampai K.

Seluruh bagian kiri bisa langsung kita abaikan.

Sekarang ruang pencarian menjadi:
```
[L .......... Z]
      ^
   buka bagian tengah lagi
```

Kita kembali membuka halaman di tengah.

Misalnya kali ini halaman tersebut berisi nama-nama yang diawali huruf R.

Karena S masih berada setelah R, kita kembali mengabaikan seluruh halaman dari L sampai R.

Ruang pencarian sekarang menjadi:
```
[S ...... Z]
    ^
 buka tengah lagi
```

Misalnya kali ini halaman tengah berisi huruf V.

Karena S berada sebelum V, kita tahu bahwa Sam tidak mungkin berada pada halaman V sampai Z.

Kita cukup mencari pada bagian kiri.
```
[S ... U]
```

Kita ulangi proses yang sama:

1. Buka bagian tengah.
2. Bandingkan dengan nilai yang dicari.
3. Buang setengah bagian yang pasti tidak mungkin berisi data.
4. Ulangi hingga data ditemukan atau tidak ada lagi ruang pencarian.

Kenapa Binary Search Sangat Cepat?

Setiap langkah selalu membuang setengah dari data yang tersisa.

Misalnya ada 1.000 halaman.

* Langkah 1 → tersisa 500 halaman.
* Langkah 2 → tersisa 250 halaman.
* Langkah 3 → tersisa 125 halaman.
* Langkah 4 → tersisa sekitar 62 halaman.
* Langkah 5 → tersisa sekitar 31 halaman.
* …
* Hanya sekitar 10 langkah untuk menemukan halaman yang dicari.

Bandingkan dengan pencarian biasa (Linear Search), yang dalam kasus terburuk bisa saja harus memeriksa hampir semua 1.000 halaman.

Inti Binary Search

Binary Search bekerja dengan prinsip sederhana:

* Ambil elemen di tengah.
* Bandingkan dengan nilai yang dicari.
* Jika nilai yang dicari lebih kecil, cari di bagian kiri.
* Jika lebih besar, cari di bagian kanan.
* Ulangi hingga data ditemukan.

Karena setiap langkah membuang setengah data, Binary Search memiliki kompleksitas waktu O(log n), sehingga jauh lebih efisien daripada Linear Search yang memiliki kompleksitas O(n).

Syarat utama Binary Search: data harus sudah terurut (sorted). Tanpa data yang terurut, kita tidak bisa menentukan apakah harus mencari ke kiri atau ke kanan.

## Contoh Binary Search dengan Daftar Angka

Misalkan kita memiliki daftar angka yang sudah diurutkan:

[3, 7, 12, 18, 25, 31, 42, 56, 68, 75, 89]

Kita ingin mencari angka 42.

### Langkah 1

Ambil elemen yang berada di tengah.
```
[3, 7, 12, 18, 25, 31, 42, 56, 68, 75, 89]
                 ^
                31
```

Nilai tengah adalah 31.

Karena 42 > 31, maka kita tahu bahwa angka 42 tidak mungkin berada di sebelah kiri.

Kita langsung mengabaikan setengah bagian kiri.

### Langkah 2

Sekarang ruang pencarian menjadi:
```
[42, 56, 68, 75, 89]
         ^
        68
```

Nilai tengah sekarang adalah 68.

Karena 42 < 68, maka kita mengabaikan seluruh bagian kanan.

### Langkah 3

Ruang pencarian menjadi:
```
[42, 56]
 ^
42
```

Nilai tengah adalah 42.

Karena nilainya sama dengan angka yang dicari, pencarian selesai.

Proses yang Terjadi
```
[3, 7, 12, 18, 25, 31, 42, 56, 68, 75, 89]
                 ↑
                31
42 > 31 → cari ke kanan
[42, 56, 68, 75, 89]
         ↑
        68
42 < 68 → cari ke kiri
[42, 56]
 ↑
42
42 == 42 → ditemukan
```

Dalam contoh ini, kita hanya membutuhkan 3 kali perbandingan untuk menemukan angka 42, padahal terdapat 11 angka dalam daftar.

Inilah alasan Binary Search jauh lebih efisien dibandingkan memeriksa angka satu per satu (Linear Search).

## Contoh Implementasi binary search menggunakan Go

```
func binarySearch(arr []int, target int) *int {
    low := 0
    high := len(arr)-1

    for low <= high {
        mid := (low + high) / 2

        if arr[mid] == target {
            return &mid
        } else if arr[mid] > target {
            high = mid - 1
        } else {
            low = mid + 1
        }
    }

    return nil
}
```

testing:
```
func main() {
    arr := []int{3, 7, 12, 18, 25, 31, 42, 56, 68, 75, 89}
	found := binarySearch(arr, 42)
	if found != nil {
		fmt.Println(*found)
	}
}
```
hasil:
```
6
```
Ditemukan angka 42 pada posisi index 6 dalam array.