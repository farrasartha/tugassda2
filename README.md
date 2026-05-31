# tugassda2
/*
========================================================
Nama Kelompok : Kelompok 12

Mata Kuliah   : Praktikum Struktur Data dan Algoritma

Nama Anggota  : 1. Muhammad Isyraq Akramin
                2. Auzan Valy Hafidza
                3. Farras Arthada Siregar
=======================================================                

=======================================================
TUGAS 2 STRUKTUR DATA DAN ALGORITMA
Implementasi dan Analisis Algoritma Sorting 
(Simple Sorting & Advanced Sorting)
=======================================================

=======================================================
Simple Sorting:
1. Bubble Sort
2. Insertion Sort
3. Selection Sort

Advanced Sorting:
1. Merge Sort (words.txt)
2. Quick Sort (words.txt)
3. Shell Sort (words.txt)
=======================================================
*/

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>

#define SIZE 10000
#define MAX_WORDS 50000
#define MAX_LEN 50

/* 
==========================
   KUMPULAN DATA STRING
========================== 
*/

char words[MAX_WORDS][MAX_LEN];
int wordCount = 0;

/* 
=========================
     VALIDASI INPUT
========================= 
*/

int inputInteger()
{
    char buffer[100];
    int angka;

    while (1)
    {
        if (!fgets(buffer, sizeof(buffer), stdin))
            return 3;

        if (sscanf(buffer, "%d", &angka) == 1)
            return angka;

        printf("Input harus berupa angka! Coba lagi: ");
    }
}

/* 
=========================
    UTILITAS INTEGER
========================= 
*/

void swap(int *a, int *b)
{
    int temp = *a;
    *a = *b;
    *b = temp;
}

void generateData(int arr[], int n)
{
    for (int i = 0; i < n; i++)
    {
        arr[i] = rand() % 10000;
    }
}

void shuffleData(int arr[], int n)
{
    for (int i = n - 1; i > 0; i--)
    {
        int j = rand() % (i + 1);

        swap(&arr[i], &arr[j]);
    }
}

void tampilData(int arr[], int n)
{
    int batas = (n < 10) ? n : 10;

    for (int i = 0; i < batas; i++)
    {
        printf("%d ", arr[i]);
    }

    printf("\n");
}

