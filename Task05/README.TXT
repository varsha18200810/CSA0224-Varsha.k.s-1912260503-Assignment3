#include <stdio.h>
#include <ctype.h>
#include <stddef.h>

int word_count(const char s[])
{
    int count = 0, inside = 0;
    for (int i = 0; s[i] != '\0'; ++i) {
        if (isspace((unsigned char)s[i])) {
            inside = 0;
        } else if (!inside) {
            ++count;
            inside = 1;
        }
    }
    return count;
}

int is_palindrome(const char s[])
{
    int left = 0, right = 0;
    while (s[right] != '\0') ++right;
    --right;

    while (left < right) {
        while (left < right &&
               !isalpha((unsigned char)s[left])) ++left;
        while (left < right &&
               !isalpha((unsigned char)s[right])) --right;
        if (tolower((unsigned char)s[left]) !=
            tolower((unsigned char)s[right])) return 0;
        ++left;
        --right;
    }
    return 1;
}

void to_upper_copy(const char src[], char dest[], size_t capacity)
{
    size_t i = 0;
    if (capacity == 0) return;
    while (src[i] != '\0' && i + 1 < capacity) {
        dest[i] = (char)toupper((unsigned char)src[i]);
        ++i;
    }
    dest[i] = '\0';
}

void reverse_copy(const char src[], char dest[], size_t capacity)
{
    size_t len = 0, i = 0;
    if (capacity == 0) return;
    while (src[len] != '\0') ++len;
    while (i < len && i + 1 < capacity) {
        dest[i] = src[len - 1 - i];
        ++i;
    }
    dest[i] = '\0';
}

int main(void)
{
    char upper[100], reversed[100];
    const char message[] = " Meet at the library at 5 PM ";
    to_upper_copy(message, upper, sizeof upper);
    reverse_copy(message, reversed, sizeof reversed);

    printf("Words: %d\n", word_count(message));
    printf("Uppercase: %s\n", upper);
    printf("Reversed: %s\n", reversed);
    printf("Palindrome results: %d %d\n",
           is_palindrome("Madam, I'm Adam"), is_palindrome("Hello"));
    printf("Tab-separated words: %d\n", word_count("one\ttwo"));
    return 0;
}

