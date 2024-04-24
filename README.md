#include <stdio.h>
#include <stdlib.h>
#include <limits.h>

#define N 5 // Number of cities

// Function to calculate the distance between two cities
int distance(int city1, int city2, int graph[N][N]) {
    return graph[city1][city2];
}

// Function to swap two cities in a tour
void swap(int tour[N], int i, int j) {
    int temp = tour[i];
    tour[i] = tour[j];
    tour[j] = temp;
}

// Function to calculate total distance of the tour
int tour_distance(int tour[N], int graph[N][N]) {
    int total_distance = 0;
    for (int i = 0; i < N - 1; i++) {
        total_distance += distance(tour[i], tour[i+1], graph);
    }
    total_distance += distance(tour[N-1], tour[0], graph); // Return to starting city
    return total_distance;
}

// 2-opt algorithm for local optimization
void two_opt(int tour[N], int graph[N][N]) {
    int improvement = 1;
    while (improvement) {
        improvement = 0;
        for (int i = 1; i < N - 1; i++) {
            for (int j = i + 1; j < N; j++) {
                int delta = distance(tour[i-1], tour[j], graph) + distance(tour[i], tour[j-1], graph)
                            - distance(tour[i-1], tour[i], graph) - distance(tour[j-1], tour[j], graph);
                
                if (delta < 0) {
                    swap(tour, i, j);
                    improvement = 1;
                }
            }
        }
    }
}

int main() {
    int graph[N][N] = {
        {0, 10, 15, 20, 25},
        {10, 0, 35, 25, 30},
        {15, 35, 0, 30, 10},
        {20, 25, 30, 0, 5},
        {25, 30, 10, 5, 0}
    };

    int tour[N] = {0, 1, 2, 3, 4}; // Starting tour: [0, 1, 2, 3, 4]

    printf("Initial Tour: ");
    for (int i = 0; i < N; i++) {
        printf("%d ", tour[i]);
    }
    printf("\n");

    printf("Initial Tour Distance: %d\n", tour_distance(tour, graph));

    two_opt(tour, graph);

    printf("Optimized Tour: ");
    for (int i = 0; i < N; i++) {
        printf("%d ", tour[i]);
    }
    printf("\n");

    printf("Optimized Tour Distance: %d\n", tour_distance(tour, graph));

    return 0;
