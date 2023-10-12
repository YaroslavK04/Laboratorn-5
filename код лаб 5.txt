#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>
#include<iostream>
#include<time.h>
#include<Windows.h>
using namespace std;

int* connect_graf;

void output(int size) {
	int schet = 0, schet1 = 0, schet2 = 0;

	for (int i = 0; i < size; i++) {
		cout << i + 1 << " - " << connect_graf[i];
		if (connect_graf[i] == size - 1) {
			cout << " - доминирующий \n";
			schet++;
		}
		else if (connect_graf[i] == 1) {
			cout << " - концевой \n";
			schet1++;
		}
		else if (connect_graf[i] == 0) {
			cout << " - изолированный \n";
			schet2++;
		}
		else { cout << "\n"; }
	}

	if (schet == 0) { cout << "Доминирующих нет\n"; }
	if (schet1 == 0) { cout << "Концевых нет\n"; }
	if (schet2 == 0) { cout << "Изолированных нет\n"; }
}

int main() {
	setlocale(LC_ALL, "Rus");
	srand(time(NULL));
	int** G,**G_inc, size, size_graf = 0, rebro = 0, size_graf_inc = 0;
	cout << " Матрица смежности \n \n Введите количество вершин графа: ";
	cin >> size;
	G = new int* [size]; // создаём двумерный массив 
	connect_graf = new int[size];
	for (int i = 0; i < size; i++) {
		G[i] = new int[size];
		connect_graf[i] = 0;
	}


	for (int i = 0; i < size; i++) {
		for (int j = i; j < size; j++) {
			if (i == j) { G[i][j] = 0; }
			else { 
				G[i][j] = rand() % 2;
				G[j][i] = G[i][j];
			}
			if (G[i][j] == 1) { size_graf++; }
		}
		
	}

	for (int i = 0; i < size; i++) {
		for (int j = 0; j < size; j++) {
			if (G[i][j] == 1) { connect_graf[i]++; }
			cout << G[i][j] << " ";
		}
		cout << "\n";
	}

	cout << "\n Размер графа = " << size_graf << "\n\n";
	
	output(size);

	cout << "\nМатрица инцидентности\n" << endl;
	G_inc = new int* [size_graf]; // создаём двумерный массив 
	for (int i = 0; i < size_graf; i++) {
		G_inc[i] = new int[size];
	}


	for (int i = 0; i < size; i++) {
		for (int j = i; j < size; j++) {
			if (G[i][j] == 1) {
				G_inc[rebro][j] = rebro + 1;
				G_inc[rebro][i] = rebro + 1;
				rebro++;
				size_graf_inc++;
			}
		}
	}

	for (int i = 0; i < size_graf; i++) {
		for (int j = 0; j < size; j++) {
			if (G_inc[i][j] < 0) {
				G_inc[i][j] = 0;
			}
			cout << G_inc[i][j] << " ";
		}
		cout << "\n";
	}
	cout << "\n Размер графа = " << size_graf_inc << "\n\n";

	for (int i = 0; i < size; i++) {
		connect_graf[i] = 0;
		for (int j = 0; j < size_graf; j++) {
			if (G_inc[j][i] >0) { connect_graf[i]++; }
		}
	}

	output(size);


	system("pause");
	return 0;
}
