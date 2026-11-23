#include <stdio.h>

int merge_desc(const int a[], int na, const int b[], int nb, int out[])
{
    int i = 0, j = 0, k = 0;
    while (i < na && j < nb) {
        if (a[i] >= b[j]) out[k++] = a[i++];
        else out[k++] = b[j++];
    }
    while (i < na) out[k++] = a[i++];
    while (j < nb) out[k++] = b[j++];
    return k;
}

int main(void)
{
    const int campusA[] = {98, 91, 85, 72};
    const int campusB[] = {95, 91, 80};
    int merged[7];
    int n = merge_desc(campusA, 4, campusB, 3, merged);
    for (int i = 0; i < n; ++i) printf("%d ", merged[i]);
    puts("");

    /* Empty-list test */
    {
        const int empty[1] = {0};
        int result[3];
        const int other[] = {90, 80, 70};
        int count = merge_desc(empty, 0, other, 3, result);
        printf("Empty-list test: ");
        for (int i = 0; i < count; ++i) printf("%d ", result[i]);
        puts("");
    }
    return 0;
}

