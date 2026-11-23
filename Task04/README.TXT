#include <stdio.h>

#define DISTRICTS 38
#define MONTHS 12

int read_rain(double rain[DISTRICTS][MONTHS])
{
    for (int i = 0; i < DISTRICTS; ++i) {
        for (int j = 0; j < MONTHS; ++j) {
            printf("Rainfall for district %d month %d: ", i + 1, j + 1);
            if (scanf("%lf", &rain[i][j]) != 1 || rain[i][j] < 0.0) {
                puts("Invalid rainfall input.");
                return 0;
            }
        }
    }
    return 1;
}

void annual_totals(const double rain[DISTRICTS][MONTHS],
                   double totals[DISTRICTS])
{
    for (int i = 0; i < DISTRICTS; ++i) {
        totals[i] = 0.0;
        for (int j = 0; j < MONTHS; ++j) totals[i] += rain[i][j];
    }
}

void monthly_averages(const double rain[DISTRICTS][MONTHS],
                      double averages[MONTHS])
{
    for (int j = 0; j < MONTHS; ++j) {
        averages[j] = 0.0;
        for (int i = 0; i < DISTRICTS; ++i) averages[j] += rain[i][j];
        averages[j] /= DISTRICTS;
    }
}

void maximum_cell(const double rain[DISTRICTS][MONTHS],
                  double *maximum, int *district, int *month)
{
    *maximum = rain[0][0]; *district = 0; *month = 0;
    for (int i = 0; i < DISTRICTS; ++i)
        for (int j = 0; j < MONTHS; ++j)
            if (rain[i][j] > *maximum) {
                *maximum = rain[i][j]; *district = i; *month = j;
            }
}

void list_deficient(const double rain[DISTRICTS][MONTHS])
{
    double grand_total = 0.0;
    for (int i = 0; i < DISTRICTS; ++i)
        for (int j = 0; j < MONTHS; ++j) grand_total += rain[i][j];

    double overall_monthly_average = grand_total / (DISTRICTS * MONTHS);
    printf("Districts below 60%% of overall monthly average (%.2f mm): ",
           0.60 * overall_monthly_average);
    for (int i = 0; i < DISTRICTS; ++i) {
        double total = 0.0;
        for (int j = 0; j < MONTHS; ++j) total += rain[i][j];
        if (total / MONTHS < 0.60 * overall_monthly_average)
            printf("%d ", i + 1);
    }
    puts("");
}

int main(void)
{
    double rain[DISTRICTS][MONTHS];
    double totals[DISTRICTS], averages[MONTHS], maximum;
    int max_district, max_month;

    if (!read_rain(rain)) return 1;
    annual_totals(rain, totals);
    monthly_averages(rain, averages);
    maximum_cell(rain, &maximum, &max_district, &max_month);

    printf("Annual total for district 1: %.2f mm\n", totals[0]);
    printf("State average for month 1: %.2f mm\n", averages[0]);
    printf("Wettest cell: district %d, month %d = %.2f mm\n",
           max_district + 1, max_month + 1, maximum);
    list_deficient(rain);
    return 0;
}

