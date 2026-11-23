#include <stdio.h>

static int linear_steps, binary_steps;

int linear_search(const int a[], int n, int key)
{
    linear_steps = 0;
    for (int i = 0; i < n; ++i) {
        ++linear_steps;
        if (a[i] == key) return i;
    }
    return -1;
}

int binary_search(const int a[], int n, int key)
{
    int low = 0, high = n - 1;
    binary_steps = 0;
    while (low <= high) {
        int mid = low + (high - low) / 2;
        ++binary_steps;
        if (a[mid] == key) return mid;
        if (a[mid] < key) low = mid + 1;
        else high = mid - 1;
    }
    return -1;
}

int main(void)
{
    const int a[] = {1021, 1048, 1103, 1190, 1222,
                     1305, 1377, 1402, 1460, 1533};
    const int keys[] = {1021, 1305, 1533, 1400};
    const int n = (int)(sizeof a / sizeof a[0]);

    for (int k = 0; k < 4; ++k) {
        int lp = linear_search(a, n, keys[k]);
        int bp = binary_search(a, n, keys[k]);
        printf("Book %d: linear index=%d comparisons=%d; "
               "binary index=%d comparisons=%d\n",
               keys[k], lp, linear_steps, bp, binary_steps);
    }
    return 0;
}

