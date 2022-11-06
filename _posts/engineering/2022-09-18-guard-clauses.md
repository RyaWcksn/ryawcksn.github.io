---
layout: post
title:  "Guard Clause, why we need it?"
date:   2022-09-18 20:00:00 +0700
tags: ["engineering"]
categories: engineering
---

# Guard Clause

Guard clause singkatnya menggunakan if else statement tanpa harus menggunakan else pada saat percabangan  

Positifnya dari implementasi Guard Clause adalah waktu eksekusi yang lebih cepat dan kemudahan akan membaca sebuah kode pada saat ingin di refactor, pengunaan Guard Clause juga membuat kodingan kita menjadi rapih, singkat padat dan jelas

## Implementasi Guard Clause

Teknik implementasi Guard Clause berasal dari metode [Fail-Fast](https://en.wikipedia.org/wiki/Fail-fast) yang berguna saat kondisi validasi agar langsung menghentikan aplikasi ketika tidak sesuai kondisi dan langsung memunculkan pesan error

**Contoh penggunaan Guard Clause**

```go
func validatePerson(name string, age int) (bool, error) {
    if age != 0 {
        if name == "" {
            return false, fmt.Errorf("name not specified")
        } else if age < 18 {
            return false, fmt.Errorf("you're not legal enough")
        } else {
            return true, nil
        }
    } else {
      return false, fmt.Errorf("age not specified")
    }
    return
}
```

Pada codeblock diatas tertulis sebuah potongan fungsi yang berguna untuk menvalidasi apakah orang tersebut valid dan sudah bisa meminum alkohol, terlihat jelas banyak sekali percabangan `if else` yang ditulis diatas, apakah fungsi tersebut valid? tentu saja valid tapi apakah fungsi tersebut efektif dan mudah direfaktor? tentu jawabannya kurang efektif dan kurang bisa di refaktor, oleh karena itu muncul lah implementasi Guard Clause

```go
func validatePerson(name string, age int) (bool, error) {
    if age == 0 {
      return false, fmt.Errorf("age not specified")
    }
    if name == "" {
        return false, fmt.Errorf("name not specified")
    } 
    if age < 18 {
        return false, fmt.Errorf("you're not legal enough")
    }
    return true, nil
}
```

Setelah kita menerapkan Guard Clause, kemudahan dalam membaca, refaktor serta keefektifan kode kita meningkat drastis, serta eksekusi waktu program kita juga ikut meningkat berkat penerapan Guard Clause ini

## Akhir Kata

Sekian dari penerapan Guard Clause dalam kodingan kita, efektifitas dan kecepatan kodingan kita akan menjadi nilai plus oleh mata Dosen atau Boss kita, oleh karena itu kita harus terus belajar akan teknologi yang baru serta improvisasi diri sendiri menjadi Developer yang baik
