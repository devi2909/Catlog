# Catlog
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <math.h>

// Function to decode a value from a given base to decimal
long long decode_value(const char *value, int base) {
    return strtoll(value, NULL, base);
}

// Function to perform Lagrange interpolation to find the constant term c
long long lagrange_interpolation(int n, int x[], long long y[]) {
    long long c = 0;
    for (int i = 0; i < n; i++) {
        long long numerator = 1;
        long long denominator = 1;
        for (int j = 0; j < n; j++) {
            if (i != j) {
                numerator *= (-x[j]);
                denominator *= (x[i] - x[j]);
            }
        }
        c += y[i] * (numerator / denominator);
    }
    return c;
}

int main() {
    // Hardcoded input for simplicity
    int n = 4; // Number of roots
    int k = 3; // Minimum number of roots required
    int m = k - 1; // Degree of the polynomial

    // Roots in the format (x, base, encoded y-value)
    int x[] = {1, 2, 3, 6};
    int base[] = {10, 2, 10, 4};
    const char *encoded_y[] = {"4", "111", "12", "213"};

    // Decode y-values
    long long y[n];
    for (int i = 0; i < n; i++) {
        y[i] = decode_value(encoded_y[i], base[i]);
        printf("Decoded y[%d] = %lld\n", x[i], y[i]);
    }

    // Ensure we have at least k points
    if (n < k) {
        printf("Error: Not enough points to reconstruct the polynomial.\n");
        return 1;
    }

    // Use Lagrange interpolation to find the constant term c
    long long c = lagrange_interpolation(k, x, y);
    printf("The secret (constant term c) is: %lld\n", c);

    return 0;
} 
OUTPUT:
Decoded y[1] = 4
Decoded y[2] = 7
Decoded y[3] = 12
Decoded y[6] = 39
The secret (constant term c) is: 3
OUTPUT FOR TEST CASE 1:
Decoded y[1] = 4 
Decoded y[2] = 7 
Decoded y[3] = 12
Decoded y[6] = 39
The secret (constant term c) is: 3
OUTPUT FOR TEST CASE 2:
Decoded y[1] = 13444211440455345511 Decoded y[2] = 123
