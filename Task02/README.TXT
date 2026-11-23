#include <stdio.h>

#define N 8

void print_array(const int a[], int n)
{
    for (int i = 0; i < n; ++i) printf("%d ", a[i]);
    puts("");
}

void bubble_sort(int a[], int n, long *cmp, long *swaps)
{
    *cmp = *swaps = 0;
    for (int end = n - 1; end > 0; --end) {
        int changed = 0;
        for (int i = 0; i < end; ++i) {
            ++*cmp;
            if (a[i] > a[i + 1]) {
                int t = a[i]; a[i] = a[i + 1]; a[i + 1] = t;
                ++*swaps; changed = 1;
            }
        }
        if (!changed) break;
    }
}

void selection_sort(int a[], int n, long *cmp, long *swaps)
{
    *cmp = *swaps = 0;
    for (int i = 0; i < n - 1; ++i) {
        int min = i;
        for (int j = i + 1; j < n; ++j) {
            ++*cmp;
            if (a[j] < a[min]) min = j;
        }
        if (min != i) {
            int t = a[i]; a[i] = a[min]; a[min] = t; ++*swaps;
        }
    }
}

void insertion_sort(int a[], int n, long *cmp, long *shifts)
{
    *cmp = *shifts = 0;
    for (int i = 1; i < n; ++i) {
        int key = a[i], j = i - 1;
        while (j >= 0) {
            ++*cmp;
            if (a[j] <= key) break;
            a[j + 1] = a[j]; ++*shifts; --j;
        }
        a[j + 1] = key;
    }
}

int main(void)
{
    int original[N] = {42, 18, 35, 12, 29, 7, 50, 21};
    int a[N]; long c, s;
    for (int i = 0; i < N; ++i) a[i] = original[i];
    bubble_sort(a, N, &c, &s);
    printf("Bubble: "); print_array(a, N);
    printf("Comparisons=%ld swaps=%ld\n", c, s);

    for (int i = 0; i < N; ++i) a[i] = original[i];
    selection_sort(a, N, &c, &s);
    printf("Selection: "); print_array(a, N);
    printf("Comparisons=%ld swaps=%ld\n", c, s);

    for (int i = 0; i < N; ++i) a[i] = original[i];
    insertion_sort(a, N, &c, &s);
    printf("Insertion: "); print_array(a, N);
    printf("Comparisons=%ld shifts=%ld\n", c, s);
    return 0;
}

