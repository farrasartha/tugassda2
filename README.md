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

#define SIZE 1000
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

/* 
=========================
    UTILITAS STRING
========================= 
*/

void swapWords(char a[], char b[])
{
    char temp[MAX_LEN];

    strcpy(temp, a);
    strcpy(a, b);
    strcpy(b, temp);
}

int loadWords(const char *filename)
{
    FILE *fp = fopen(filename, "r");

    if (fp == NULL)
    {
        printf("\nERROR: words.txt tidak ditemukan!\n");
        return 0;
    }

    wordCount = 0;

    while (
        wordCount < MAX_WORDS &&
        fscanf(fp, "%49s", words[wordCount]) == 1
    )
    {
        wordCount++;
    }

    fclose(fp);

    return wordCount;
}

void shuffleWords()
{
    for (int i = wordCount - 1; i > 0; i--)
    {
        int j = rand() % (i + 1);

        swapWords(words[i], words[j]);
    }
}

void tampilWords()
{
    int batas =
        (wordCount < 10)
        ? wordCount
        : 10;

    for (int i = 0; i < batas; i++)
    {
        printf("%s\n", words[i]);
    }
}

/* 
=========================
     SIMPLE SORTING
========================= 
*/

void bubbleSort(int arr[], int n)
{
    for (int i = 0; i < n - 1; i++)
    {
        for (int j = 0; j < n - i - 1; j++)
        {
            if (arr[j] > arr[j + 1])
            {
                swap(&arr[j], &arr[j + 1]);
            }
        }
    }
}

void insertionSort(int arr[], int n)
{
    for (int i = 1; i < n; i++)
    {
        int key = arr[i];
        int j = i - 1;

        while (j >= 0 && arr[j] > key)
        {
            arr[j + 1] = arr[j];
            j--;
        }

        arr[j + 1] = key;
    }
}

void selectionSort(int arr[], int n)
{
    for (int i = 0; i < n - 1; i++)
    {
        int min = i;

        for (int j = i + 1; j < n; j++)
        {
            if (arr[j] < arr[min])
            {
                min = j;
            }
        }

        swap(&arr[i], &arr[min]);
    }
}

/* 
================================
   MENJALANKAN SIMPLE SORTING
================================ 
*/

void runSimpleSorting(
    void (*sortFunc)(int[], int),
    const char *nama)
{
    int arr[SIZE];

    generateData(arr, SIZE);

    shuffleData(arr, SIZE);

    printf("\n=== %s ===\n", nama);

    printf("\n10 Data Sebelum Sorting:\n\n");

    tampilData(arr, SIZE);

    clock_t start = clock();

    sortFunc(arr, SIZE);

    clock_t end = clock();

    printf("\n10 Data Sesudah Sorting:\n\n");

    tampilData(arr, SIZE);

    double waktu =
        (double)(end - start)
        / CLOCKS_PER_SEC;

    printf(
        "\nWaktu Eksekusi : %.12f detik\n",
        waktu
    );
}
/* 
=============================
      ADVANCED SORTING
=============================

=============================
   MERGE SORT UNTUK STRING
============================= 
*/

void merge(char arr[][MAX_LEN], int left, int mid, int right)
{
    int n1 = mid - left + 1;
    int n2 = right - mid;

    char L[n1][MAX_LEN];
    char R[n2][MAX_LEN];

    for (int i = 0; i < n1; i++)
        strcpy(L[i], arr[left + i]);

    for (int j = 0; j < n2; j++)
        strcpy(R[j], arr[mid + 1 + j]);

    int i = 0;
    int j = 0;
    int k = left;

    while (i < n1 && j < n2)
    {
        if (strcmp(L[i], R[j]) <= 0)
        {
            strcpy(arr[k], L[i]);
            i++;
        }
        else
        {
            strcpy(arr[k], R[j]);
            j++;
        }
        k++;
    }

    while (i < n1)
    {
        strcpy(arr[k], L[i]);
        i++;
        k++;
    }

    while (j < n2)
    {
        strcpy(arr[k], R[j]);
        j++;
        k++;
    }
}

void mergeSortWords(char arr[][MAX_LEN], int left, int right)
{
    if (left < right)
    {
        int mid = left + (right - left) / 2;

        mergeSortWords(arr, left, mid);
        mergeSortWords(arr, mid + 1, right);

        merge(arr, left, mid, right);
    }
}

/* 
==============================
   MENJALANKAN MERGE SORT
============================== 
*/

void runMergeSortWords()
{
    if (!loadWords("words.txt"))
        return;

    shuffleWords();

    printf("\n=== MERGE SORT ===\n");

    printf("\n10 Kata Sebelum Sorting:\n\n");
    tampilWords();

    clock_t start = clock();

    mergeSortWords(
        words,
        0,
        wordCount - 1
    );

    clock_t end = clock();

    printf("\n10 Kata Sesudah Sorting:\n\n");
    tampilWords();

    double waktu =
        (double)(end - start)
        / CLOCKS_PER_SEC;

    printf(
        "\nWaktu Eksekusi : %.12f detik\n",
        waktu
    );
}

/* 
=============================
   QUICK SORT UNTUK STRING
============================= 
*/

int partitionWords(
    char arr[][MAX_LEN],
    int low,
    int high)
{
    char pivot[MAX_LEN];

    strcpy(pivot, arr[high]);

    int i = low - 1;

    for (int j = low; j < high; j++)
    {
        if (strcmp(arr[j], pivot) < 0)
        {
            i++;

            swapWords(
                arr[i],
                arr[j]
            );
        }
    }

    swapWords(
        arr[i + 1],
        arr[high]
    );

    return i + 1;
}

void quickSortWords(
    char arr[][MAX_LEN],
    int low,
    int high)
{
    if (low < high)
    {
        int pi =
            partitionWords(
                arr,
                low,
                high
            );

        quickSortWords(
            arr,
            low,
            pi - 1
        );

        quickSortWords(
            arr,
            pi + 1,
            high
        );
    }
}

/* 
============================
   MENJALANKAN QUICK SORT
============================ 
*/

void runQuickSortWords()
{
    if (!loadWords("words.txt"))
        return;

    shuffleWords();

    printf("\n=== QUICK SORT ===\n");

    printf("\n10 Kata Sebelum Sorting:\n\n");

    tampilWords();

    clock_t start = clock();

    quickSortWords(
        words,
        0,
        wordCount - 1
    );

    clock_t end = clock();

    printf("\n10 Kata Sesudah Sorting:\n\n");

    tampilWords();

    double waktu =
        (double)(end - start)
        / CLOCKS_PER_SEC;

    printf(
        "\nWaktu Eksekusi : %.12f detik\n",
        waktu
    );
}
/* 
=============================
   SHELL SORT UNTUK STRING
============================= 
*/

void shellSortWords(
    char arr[][MAX_LEN],
    int n)
{
    for (int gap = n / 2;
         gap > 0;
         gap /= 2)
    {
        for (int i = gap;
             i < n;
             i++)
        {
            char temp[MAX_LEN];

            strcpy(
                temp,
                arr[i]
            );

            int j;

            for (
                j = i;
                j >= gap &&
                strcmp(
                    arr[j - gap],
                    temp
                ) > 0;
                j -= gap
            )
            {
                strcpy(
                    arr[j],
                    arr[j - gap]
                );
            }

            strcpy(
                arr[j],
                temp
            );
        }
    }
}

/* 
============================
   MENJALANKAN SHELL SORT
============================ 
*/

void runShellSortWords()
{
    if (!loadWords("words.txt"))
        return;

    shuffleWords();

    printf("\n=== SHELL SORT ===\n");

    printf("\n10 Kata Sebelum Sorting:\n\n");

    tampilWords();

    clock_t start = clock();

    shellSortWords(
        words,
        wordCount
    );

    clock_t end = clock();

    printf("\n10 Kata Sesudah Sorting:\n\n");

    tampilWords();

    double waktu =
        (double)(end - start)
        / CLOCKS_PER_SEC;

    printf(
        "\nWaktu Eksekusi : %.12f detik\n",
        waktu
    );
}

/* 
=========================
   MENU SIMPLE SORTING
========================= 
*/

void menuSimpleSorting()
{
    int pilih;

    do
    {
        printf("\n=================================\n");
        printf("         SIMPLE SORTING\n");
        printf("==================================\n");

        printf("1. Bubble Sort\n");
        printf("2. Insertion Sort\n");
        printf("3. Selection Sort\n");
        printf("4. Kembali\n");

        printf("\nPilih Metode : ");

        pilih = inputInteger();

        switch (pilih)
        {
            case 1:
                runSimpleSorting(
                    bubbleSort,
                    "Bubble Sort"
                );
                break;

            case 2:
                runSimpleSorting(
                    insertionSort,
                    "Insertion Sort"
                );
                break;

            case 3:
                runSimpleSorting(
                    selectionSort,
                    "Selection Sort"
                );
                break;

            case 4:
                printf("\nKembali ke Main Menu...\n");
                break;

            default:
                printf(
                    "\nMenu tidak tersedia!\n"
                );
        }

    }
    while (pilih != 4);
}

/* 
===========================
   MENU ADVANCED SORTING
=========================== 
*/

void menuAdvancedSorting()
{
    int pilih;

    do
    {
        printf("\n=================================\n");
        printf("        ADVANCED SORTING\n");
        printf("==================================\n");

        printf("1. Merge Sort\n");
        printf("2. Quick Sort\n");
        printf("3. Shell Sort\n");
        printf("4. Kembali\n");

        printf("\nPilih Metode : ");

        pilih = inputInteger();

        switch (pilih)
        {
            case 1:
                runMergeSortWords();
                break;

            case 2:
                runQuickSortWords();
                break;

            case 3:
                runShellSortWords();
                break;

            case 4:
                printf("\nKembali ke Main Menu...\n");
                break;

            default:
                printf("\nMenu tidak tersedia!\n");
        }

    }
    while (pilih != 4);
}

/* 
=========================
      PROGRAM UTAMA
========================= 
*/

int main()
{
    srand((unsigned)time(NULL));

    int pilihan;

    do
    {
        printf("\n=================================\n");
        printf("            MAIN MENU\n");
        printf("=================================\n");

        printf("1. Simple Sorting\n");
        printf("2. Advanced Sorting\n");
        printf("3. Exit\n");

        printf("\nPilih Menu : ");

        pilihan = inputInteger();

        switch (pilihan)
        {
            case 1:
                menuSimpleSorting();
                break;

            case 2:
                menuAdvancedSorting();
                break;

            case 3:
                printf("\nProgram selesai.\n");
                break;

            default:
                printf("\nMenu tidak tersedia!\n");
        }

    }
    while (pilihan != 3);

    return 0;
}
