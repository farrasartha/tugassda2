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
