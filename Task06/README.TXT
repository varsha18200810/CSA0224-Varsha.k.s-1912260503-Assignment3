#include <stdio.h>
#include <string.h>

#define MAX_NAMES 500
#define NAME_LEN 30

void remove_newline(char s[])
{
    size_t n = strlen(s);
    if (n > 0 && s[n - 1] == '\n') s[n - 1] = '\0';
}

void sort_names(char names[][NAME_LEN], int n)
{
    char temp[NAME_LEN];
    for (int i = 1; i < n; ++i) {
        strcpy(temp, names[i]);
        int j = i - 1;
        while (j >= 0 && strcmp(names[j], temp) > 0) {
            strcpy(names[j + 1], names[j]);
            --j;
        }
        strcpy(names[j + 1], temp);
    }
}

int binary_search(char names[][NAME_LEN], int n, const char key[])
{
    int low = 0, high = n - 1;
    while (low <= high) {
        int mid = low + (high - low) / 2;
        int result = strcmp(names[mid], key);
        if (result == 0) return mid;
        if (result < 0) low = mid + 1;
        else high = mid - 1;
    }
    return -1;
}

int main(void)
{
    char names[MAX_NAMES][NAME_LEN];
    int n;
    printf("Number of residents (1-%d): ", MAX_NAMES);
    if (scanf("%d", &n) != 1 || n < 1 || n > MAX_NAMES) {
        puts("Invalid number of residents.");
        return 1;
    }
    int ch;
    while ((ch = getchar()) != '\n' && ch != EOF) { }

    for (int i = 0; i < n; ++i) {
        printf("Name %d (maximum %d characters): ", i + 1, NAME_LEN - 1);
        if (fgets(names[i], NAME_LEN, stdin) == NULL) {
            puts("Input error."); return 1;
        }
        remove_newline(names[i]);
        if (names[i][0] == '\0') {
            puts("Name cannot be empty."); return 1;
        }
    }
    sort_names(names, n);
    puts("\nSorted directory:");
    for (int i = 0; i < n; ++i) puts(names[i]);

    for (int i = 1; i < n; ++i)
        if (strcmp(names[i - 1], names[i]) == 0)
            printf("Duplicate: %s\n", names[i]);

    {
        char key[NAME_LEN];
        printf("Search name: ");
        if (fgets(key, sizeof key, stdin) == NULL) return 1;
        remove_newline(key);
        int pos = binary_search(names, n, key);
        if (pos >= 0) printf("Found at sorted index %d\n", pos);
        else puts("Name not found.");
    }
    return 0;
}

