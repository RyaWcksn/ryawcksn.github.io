---
layout: post
title:  "Do we need calculate Cognitive Complexity?"
date:   2022-09-19 01:00:00 +0700
tags: ["engineering"]
categories: engineering
---

# Cognitive Complexity

Ketika kita membuat sebuah aplikasi, maka akan ada bentuk percabangan entah mengunakan `if else` atau `swith case`, sebuah Cognitive Complexity dihitung saat adanya 1 atau lebih percabangan, kenapa kita sebagai Software Engineer perlu menerapkan perhitungan Cognitive Complexity agar kualitas kode kita baik dan mudah terbaca, serta memudahkan kita untuk melakukan refactor kode 

Beberapa hal positif dari Cognitive Complexity adalah mempermudah Engineer baru dalam memahami sebuah program, jika suatu kode susah dipahami oleh Engineer maka terjadinya sebuah bug atau error akan meningkat, dan mempermudah semua engineer untuk refactoring program tersebut

> Software engineer yang baik adalah software engineer yang dapat membuat kode yang mudah dibaca oleh manusia dan komputer

## Penerapan Cognitive Complexity

Diberikan contoh fungsi `if else` statement sebagai berikut

```go
func getTransactionStatus(transactionStatus int) string {
    var status string
    if transactionStatus == 1 { // +1
        status = "Success"
    } else if transactionStatus == 2 { // +1
        status = "On Progress"
    } else if transactionStatus == 3 { // +1
        status = "Cancel"
    } else if transactionStatus == 0 { // +1
        status = "Failed"
    }
    return status
} // Jumlah Cognitive Complexity ada 4
``` 
Code diatas bisa direfactor menggunakan `switch case` statement agar Cognitive Statement kita menjadi lebih sedikit

```go
func getTransactionStatus(transactionStatus int) string {
    var status string
    switch transactionStatus { // +1
        case 1: 
            status = "Success"
        case 2:
            status = "On Progress"
        case 3:
            status = "Cancel"
        case 0:
            status = "Failed"
    }
    return status
} // Jumlah Cognitive Complexity ada 1
``` 

Jika kita melihat kode diatas, pemahaman kita akan fungsi tersebut menjadi mudah oleh karena itu Cognitive Complexity dari fungsi tersebut menjadi rendah

Beberapa hal yang perlu diketahui dalam Cognitive Complexity adalah nested function seperti contoh dibawah ini

```go
func validatePerson(name string, age int) (bool, error) {
    if age != 0 { // +1
        if name == "" { // +2 (nested)
            return false, fmt.Errorf("name not specified")
        } else if age < 18 { // +2 (nested)
            return false, fmt.Errorf("you're not legal enough")
        } else { // +2 (nested)
            return true, nil
        }
    } else { // +1
      return false, fmt.Errorf("age not specified")
    }
    return
} // Jumlah Cognitive Complexity ada 8
```
Jika kita menggabungkan Guard Clause dan Cognitive Complexity akan menjadi seperti ini

```go
func validatePerson(name string, age int) (bool, error) {
    if age == 0 { // +1
      return false, fmt.Errorf("age not specified")
    }
    if name == "" { // +1
        return false, fmt.Errorf("name not specified")
    } 
    if age < 18 { // +1
        return false, fmt.Errorf("you're not legal enough")
    }
    return true, nil
} // Jumlah Cognitive Complexity ada 3
```

Jumlah Cognitive Complexity pada 2 code block diatas sangat jauh berberda karena kita tidak memasukan terlalu banyak percabangan dan nesting dalam fungsi tersebut sehingga kode yang kita buat menjadi lebih mudah dibaca dan dipahami bahkan oleh engineer pemula sekalipun


## Rules

Dalam Cognitive Complexity ada beberapa rules yang harus dipahami, setiap bahasa pemrograman memiliki rules yang berbeda beda, karena aku menggunakan Golang, rules yang ada di catatan ini akan berpacu kepada Golang

Setiap operasi  
    1. `if`, `else if`, `else`  
    2. `switch`  
    3. `for`  
    dst

Akan menambah 1 Cognitive Complexity

Dan jika didalam operasi diatas ada operasi percabangan lagi maka dihitung menjadi 2 seperti contoh

```go
if a == b { // +1
    if a == c { // +2
        if a == d { // +3
            
        }
    }
} // Total menjadi 6 Cognitive Complexity
```
berlipat 1,2,3 dan seterusnya semakin dalam nesting semakin besar juga Cognitive Complexity-nya

## Cara mengatasi Cognitive Complexity

Untuk menghindari Cognitive Complexity yang terlalu besar bisa dengan menguragi operasi percabangan, biasanya dengan cara menambah fungsi baru dengan spesifikasi tertentu

```go
func transactionCreditCard(age int, name string, amount int, isRegistered bool) (bool, error) {

    validatePerson, err := validatePerson(age, name)
    if err != nil {
        return false, err
    }

    validateTransaction, err := validateTransaction(amount, isRegistered)
    if err != nil {
        return false, err
    }

    return true, nil
}

func validatePerson(age int, name string) (bool, error) {
    if age < 21 {
        return false, fmt.Errorf("not legal enough")
    }
    if name == "" {
        return false, fmt.Errorf("name not specified")
    }
    return true, nil
}

func validateTransaction(amount int, isRegistered bool) (bool, error) {
    if amount > 2000000000 {
        return false, fmt.Errorf("amount cannot exceed 2 billion")
    }
    if isRegistered == false {
        return false, fmt.Errorf("please register firs")
    }
    return true, nil
}
```

Codeblock diatas menunjukan pembagian validasi mengunakan fungsi baru agar mempermudah membaca kode dan mengurangi Cognitive Complexity, jika tidak dipisah-pisah maka pembacaan kode akan sulit

## Akhir kata

Sekian dari post tentang Cognitive Complexity, dengan ini kita bisa membuat kode yang efektif, efisien dan mudah terbaca untuk engineer lain yang akan onborading atau mengubah kode yang telah kita buat, jangan pernah membuat susah co-worker kita dengan kode kita yang susah dipahami karena hidup mereka sudah susah jangan ditambah susah lagi


