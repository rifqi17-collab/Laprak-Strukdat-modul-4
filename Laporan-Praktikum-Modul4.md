# <h1 align="center">Laporan Praktikum Modul 4 - Singly Linked List (Bagian Pertama)</h1>
<p align="center">Muh rifqi irwanto - 109082500016</p>

## Dasar Teori
Single Linked List adalah model ADT Linked List yang hanya memiliki pointer satu arah. Setiap elemen (node) memiliki dua komponen: info (data yang disimpan) dan next (pointer yang mengarah ke elemen selanjutnya). Linked list bersifat dinamis karena dapat bertambah dan menyusut sesuai kebutuhan, berbeda dengan array yang statistik [1]. Beberapa istilah penting dalam Singly Linked List adalah: first/head: pointer yang menunjuk ke alamat elemen pertama list, next: pointer pada elemen yang berfungsi sebagai penerus, Nil/NULL: nilai kosong yang tidak mengarah ke mana pun, dan Node/simpul: tempat penyimpanan data di memori. Operasi dasar dalam ADT Linked List mencakup: CreateList, alokasi, dealokasi, insertFirst, insertLast, insertAfter, deleteFirst, deleteLast, deleteAfter, printInfo, dan nbList [1]. Implementasi ADT dalam C++ dilakukan dengan memisahkan file .h (header untuk prototipe dan tipe data), .cpp (realisasi primitif), dan

# 1.Program ADT Singly Linked List dengan Data Mahasiswa
Program ini adalah penerapan dari ADT Singly Linked List yang memisahkan definisi tipe dan primitif ke dalam file list. h, implementasi primitif ke dalam file list. cpp, dan program inti di main. cpp. Struktur data mencakup mahasiswa (nama, nim, umur), node yang memiliki field isidata dengan tipe dataMahasiswa dan next dengan tipe address, serta linkedList yang memiliki field pertama.

```C++
File: list.h
#ifndef LIST_H
#define LIST_H
#define Nil NULL

#include<iostream>
using namespace std;

struct mahasiswa {
    string nama;
    string nim;
    int umur;
};

typedef mahasiswa dataMahasiswa;

typedef struct node *address;

struct node {
    dataMahasiswa isidata;
    address next;
};

struct linkedList {
    address first;
};

bool isEmpty(linkedList list);
void createList(linkedList &list);
address alokasi(string nama, string nim, int umur);
void dealokasi(address &node);
void printList(linkedList list);
void insertFirst(linkedList &list, address nodeBaru);
void insertAfter(linkedList &list, address nodeBaru, address prev);
void insertLast(linkedList &list, address nodeBaru);

void delFirst(linkedList &list);
void delLast(linkedList &list);
void delAfter(linkedList &list, address nodeHapus, address nodePrev);
int nbList(linkedList list);
void deleteList(linkedList &list);

#endif

File: list.cpp
#include "list.h"
#include <iostream>
using namespace std;

//I.S = Initial State / kondisi awal
//F.S = Final State / kondisi akhir

//fungsi untuk cek apakah list kosong atau tidak
bool isEmpty(linkedList list) {
    if(list.first == Nil){
        return true;
    } else {
        return false;
    }
}

//pembuatan linked list kosong
void createList(linkedList &list) {
    /* I.S. sembarang
       F.S. terbentuk list kosong */
    list.first = Nil;
}

//pembuatan node baru dengan menerapkan manajemen memori
address alokasi(string nama, string nim, int umur) {
    /* I.S. sembarang
       F.S. mengembalikan alamat node baru dengan isidata = sesuai parameter dan next = Nil */
    address nodeBaru = new node;
    nodeBaru->isidata.nama = nama;
    nodeBaru->isidata.nim = nim;
    nodeBaru->isidata.umur = umur;
    nodeBaru->next = Nil;
    return nodeBaru;
}

//penghapusan node dengan menerapkan manajemen memori
void dealokasi(address &node) {
    /* I.S. P terdefinisi
       F.S. memori yang digunakan node dikembalikan ke sistem */
    node->next = Nil;
    delete node;
}

//prosedur-prosedur untuk insert / menambahkan node baru kedalam list
void insertFirst(linkedList &list, address nodeBaru) {
    /* I.S. sembarang, P sudah dialokasikan
       F.S. menempatkan elemen list (node) pada awal list */
    nodeBaru->next = list.first;
    list.first = nodeBaru;
}

void insertAfter(linkedList &list, address nodeBaru, address prev) {
    /* I.S. sembarang, nodeBaru dan Prev alamat salah satu elemen list (node)
       F.S. menempatkan elemen (node) sesudah elemen node Prev */
    if (prev != Nil) {
        nodeBaru->next = prev->next;
        prev->next = nodeBaru;
    } else {
        cout << "Node sebelumnya tidak valid!" << endl;
    }
}

void insertLast(linkedList &list, address nodeBaru) {
    /* I.S. sembarang, nodeBaru sudah dialokasikan
       F.S. menempatkan elemen nodeBaru pada akhir list */
    if (isEmpty(list)) {
        list.first = nodeBaru;
    } else {
        address nodeBantu = list.first;
        while (nodeBantu->next != Nil) {
            nodeBantu = nodeBantu->next;
        }
        nodeBantu->next = nodeBaru;
    }
}

//prosedur-prosedur untuk delete / menghapus node yang ada didalam list
void delFirst(linkedList &list){
    /* I.S. list tidak kosong
       F.S. node pertama di list terhapus*/
    address nodeHapus;
    if (isEmpty(list) == false) {
        nodeHapus = list.first;
        list.first = list.first->next;
        nodeHapus->next = Nil;
        dealokasi(nodeHapus);
    } else {
        cout << "List kosong!" << endl;
    }
}

void delLast(linkedList &list){
    /* I.S. list tidak kosong
       F.S. node terakhir di list terhapus */
    address nodeHapus, nodePrev;
    if(isEmpty(list) == false){
        nodeHapus = list.first;
        if(nodeHapus->next == Nil){
            list.first = Nil;
            dealokasi(nodeHapus);
        } else {
            while(nodeHapus->next != Nil){
                nodePrev = nodeHapus;
                nodeHapus = nodeHapus->next;
            }
            nodePrev->next = Nil;
            dealokasi(nodeHapus);
        }
    } else {
        cout << "list kosong" << endl;
    }
}

void delAfter(linkedList &list, address nodeHapus, address nodePrev){
    /* I.S. list tidak kosong, Prev alamat salah satu elemen list
       F.S. nodeBantu adalah alamat dari Prev->next, menghapus Prev->next dari list */
    if(isEmpty(list) == true){
        cout << "list kosong!" << endl;
    } else {
        if (nodePrev != Nil && nodePrev->next != Nil) {
            nodeHapus = nodePrev->next;
            nodePrev->next = nodeHapus->next;
            nodeHapus->next = Nil;
            dealokasi(nodeHapus);
        } else {
            cout << "Node sebelumnya (prev) tidak valid!" << endl;
        }
    }
}

//prosedur untuk menampilkan isi list
void printList(linkedList list) {
    /* I.S. list mungkin kosong
       F.S. jika list tidak kosong menampilkan semua info yang ada pada list */
    if (isEmpty(list)) {
        cout << "List kosong." << endl;
    } else {
        address nodeBantu = list.first;
        while (nodeBantu != Nil) {
            cout << "Nama : " << nodeBantu->isidata.nama << ", NIM : " << nodeBantu->isidata.nim << ", Usia : " << nodeBantu->isidata.umur << endl;
            nodeBantu = nodeBantu->next;
        }
    }
}

//function untuk menampilkan jumlah node didalam list
int nbList(linkedList list) {
    /* I.S. list sudah ada
       F.S. menampilkan jumlah node didalam list*/
    int count = 0;
    address nodeBantu = list.first;
    while (nodeBantu != Nil) {
        count++;
        nodeBantu = nodeBantu->next;
    }
    return count;
}

//prosedur untuk menghapus list (menghapus semua node didalam list)
void deleteList(linkedList &list){
    /* I.S. list sudah ada
       F.S. menghapus semua node didalam list*/
    address nodeBantu, nodeHapus;
    nodeBantu = list.first;
    while(nodeBantu != Nil){
        nodeHapus = nodeBantu;
        nodeBantu = nodeBantu->next;
        dealokasi(nodeHapus);
    }
    list.first = Nil;
    cout << "List sudah terhapus!" << endl;
}

File: main.cpp
#include "list.h"

#include<iostream>
using namespace std;

int main(){
    linkedList List;
    address nodeA, nodeB, nodeC, nodeD, nodeE = Nil;
    createList(List);

    dataMahasiswa mhs;

    nodeA = alokasi("Dhimas", "2311102151", 20);
    nodeB = alokasi("Arvin", "221110014", 21);
    nodeC = alokasi("Rizal", "231110029", 20);
    nodeD = alokasi("Satro", "2211102173", 21);
    nodeE = alokasi("Joshua", "2311102133", 21);

    insertFirst(List, nodeA);
    insertLast(List, nodeB);
    insertAfter(List, nodeC, nodeA);
    insertAfter(List, nodeD, nodeC);
    insertLast(List, nodeE);

    cout << "--- ISI LIST SETELAH DILAKUKAN INSERT ---" << endl;
    printList(List);
    cout << "Jumlah node : " << nbList(List) << endl;
    cout << endl;

    delFirst(List);
    delLast(List);
    delAfter(List, nodeD, nodeC);

    cout << "--- ISI LIST SETELAH DILAKUKAN DELETE ---" << endl;
    printList(List);
    cout << "Jumlah node : " << nbList(List) << endl;
    cout << endl;

    deleteList(List);
    cout << "--- ISI LIST SETELAH DILAKUKAN HAPUS LIST ---" << endl;

    printList(List);
    cout << "Jumlah node : " << nbList(List) << endl;
    cout << endl;

    return 0;
}
```
Program ini menunjukkan ide dasar dari ADT Singly Linked List dengan memisahkan file header, implementasi, dan pengemudi. Data yang dipakai adalah informasi mahasiswa yang mencakup nama, nim, dan usia.
## Unguided 

### 1. Program ADT Singly Linked List - Insert First

```C++

File: singlylist.h
#ifndef SINGLYLIST_H
#define SINGLYLIST_H

typedef int infotype;
typedef struct tElmtList *address;

typedef struct tElmtList {
    infotype info;
    address next;
} ElmtList;

typedef struct {
    address first;
} List;

void CreateList(List *L);
address alokasi(infotype x);
void dealokasi(address P);
void printInfo(List L);
void insertFirst(List *L, address P);

#endif

File: singlylist.cpp
#include <iostream>
#include "Singlylist.h"

using namespace std;

void CreateList(List *L) {
    L->first = NULL;
}

address alokasi(infotype x) {
    address P = new ElmtList;

    if (P != NULL) {
        P->info = x;
        P->next = NULL;
    }

    return P;
}

void dealokasi(address P) {
    delete P;
}

void printInfo(List L) {
    address P = L.first;

    while (P != NULL) {
        cout << P->info << " ";
        P = P->next;
    }

    cout << endl;
}

void insertFirst(List *L, address P) {
    if (P != NULL) {
        P->next = L->first;
        L->first = P;
    }
}

File: main.cpp
#include <iostream>
#include "Singlylist.h"

using namespace std;

int main() {
    List L;
    address P1, P2, P3, P4, P5;

    CreateList(&L);

    P1 = alokasi(2);
    insertFirst(&L, P1);

    P2 = alokasi(0);
    insertFirst(&L, P2);

    P3 = alokasi(8);
    insertFirst(&L, P3);

    P4 = alokasi(12);
    insertFirst(&L, P4);

    P5 = alokasi(9);
    insertFirst(&L, P5);

    printInfo(L);

    return 0;
}
Program membuat list kosong terlebih dahulu dengan CreateList, kemudian mengalokasikan 5 node secara berurutan dengan nilai 2, 0, 8, 12, dan 9 lalu menyisipkannya di awal list menggunakan insertFirst. Karena penyisipan selalu di awal, node yang terakhir disisipkan (9) menjadi elemen pertama list, diikuti 12, 8, 0, dan 2 sebagai elemen terakhir. Fungsi printInfo menelusuri list dari First hingga Nil dan menampilkan setiap info node sehingga output yang dihasilkan adalah 9 12 8 0 2, sesuai dengan ilustrasi linked list pada soal.
```
### 2.Program ADT Singly Linked List - Delete First, Delete Last, Delete After

```C++
File: Singlylist.h

#ifndef SINGLYLIST_H
#define SINGLYLIST_H

typedef int infotype;
typedef struct tElmtList *address;

typedef struct tElmtList {
    infotype info;
    address next;
} ElmtList;

typedef struct {
    address first;
} List;

// Fungsi dan prosedur dasar
void CreateList(List *L);
address alokasi(infotype x);
void dealokasi(address P);
void printInfo(List L);
void insertFirst(List *L, address P);

// Fungsi dan prosedur penghapusan
void deleteFirst(List *L);
void deleteLast(List *L);
void deleteAfter(List *L, address prec);
int jumlahList(List L);
void deleteList(List *L);

#endif


File: Singlylist.cpp

#include <iostream>
#include "Singlylist.h"

using namespace std;

void CreateList(List *L) {
    L->first = NULL;
}

address alokasi(infotype x) {
    address P = new ElmtList;

    if (P != NULL) {
        P->info = x;
        P->next = NULL;
    }

    return P;
}

void dealokasi(address P) {
    delete P;
}

void printInfo(List L) {
    address P = L.first;

    while (P != NULL) {
        cout << P->info << " ";
        P = P->next;
    }

    cout << endl;
}

void insertFirst(List *L, address P) {
    if (P != NULL) {
        P->next = L->first;
        L->first = P;
    }
}

void deleteFirst(List *L) {
    if (L->first != NULL) {
        address P = L->first;
        L->first = P->next;
        dealokasi(P);
    }
}

void deleteLast(List *L) {
    if (L->first == NULL) {
        return;
    }

    if (L->first->next == NULL) {
        deleteFirst(L);
        return;
    }

    address P = L->first;

    while (P->next->next != NULL) {
        P = P->next;
    }

    address Q = P->next;
    P->next = NULL;
    dealokasi(Q);
}

void deleteAfter(List *L, address prec) {
    if (prec != NULL && prec->next != NULL) {
        address P = prec->next;
        prec->next = P->next;
        dealokasi(P);
    }
}

int jumlahList(List L) {
    int jumlah = 0;
    address P = L.first;

    while (P != NULL) {
        jumlah++;
        P = P->next;
    }

    return jumlah;
}

void deleteList(List *L) {
    while (L->first != NULL) {
        deleteFirst(L);
    }
}

File: main.cpp
#include <iostream>
#include "Singlylist.h"

using namespace std;

int main() {
    List L;
    address P1, P2, P3, P4, P5;

    CreateList(&L);

    P1 = alokasi(2);
    insertFirst(&L, P1);

    P2 = alokasi(0);
    insertFirst(&L, P2);

    P3 = alokasi(8);
    insertFirst(&L, P3);

    P4 = alokasi(12);
    insertFirst(&L, P4);

    P5 = alokasi(9);
    insertFirst(&L, P5);

    deleteFirst(&L);
    deleteLast(&L);

    address prec = L.first;
    deleteAfter(&L, prec);

    printInfo(L);
    cout << "Jumlah node : " << jumlahList(L) << endl;

    deleteList(&L);

    cout << "-- List berhasil terhapus --" << endl;
    cout << "Jumlah node : " << jumlahList(L) << endl;

    return 0;
}
```
Program melanjutkan dari list hasil Soal 1 yaitu 9 → 12 → 8 → 0 → 2. Operasi deleteFirst menghapus node 9 karena berada di awal list, menghasilkan 12 → 8 → 0 → 2. Operasi deleteLast menghapus node 2 karena berada di akhir list, menghasilkan 12 → 8 → 0. Operasi deleteAfter dengan Prec = node 12 (dicari menggunakan findElm) menghapus node 8 yang berada tepat setelahnya, menghasilkan 12 → 0. Fungsi nbList menghitung jumlah node tersisa yaitu 2. Terakhir, deleteList menghapus seluruh node beserta memori yang dialokasikan sehingga list kembali kosong dengan jumlah node 0, sesuai dengan output yang diharapkan pada soal.

## Kesimpulan
Berdasarkan praktik Modul 4, dapat disimpulkan bahwa Singly Linked List merupakan struktur data yang terdiri dari sejumlah node yang terhubung satu sama lain melalui pointer next. Proses pembuatan ADT dalam C++ dibagi ke dalam tiga file, yaitu . h yang berfungsi untuk mendeklarasikan tipe data dan fungsi, . cpp untuk menerapkan berbagai operasi, serta main. cpp sebagai program intinya. Operasi insertFirst digunakan untuk menambahkan node di bagian depan sehingga urutan data menjadi terbalik dari proses pemasukkannya. Sementara itu, deleteFirst, deleteLast, dan deleteAfter berfungsi untuk menghapus node pada posisi tertentu. Setiap node yang dihapus harus didealokasi agar memori tidak terbuang sia-sia. Selain itu, fungsi nbList digunakan untuk menghitung total jumlah node dalam list, sementara deleteList berfungsi menghapus seluruh node agar list kembali dalam keadaan kosong.
