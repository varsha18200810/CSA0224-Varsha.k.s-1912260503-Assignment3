#include <stdio.h>

#define MAX_CANDIDATES 12
#define MAX_VOTES 5000

int main(void)
{
    int candidates, votes;
    int count[MAX_CANDIDATES + 1] = {0};
    int order[MAX_CANDIDATES + 1];
    int valid = 0, invalid = 0;

    printf("Number of candidates (1-%d): ", MAX_CANDIDATES);
    if (scanf("%d", &candidates) != 1 ||
        candidates < 1 || candidates > MAX_CANDIDATES) {
        puts("Invalid candidate count."); return 1;
    }
    printf("Number of votes (0-%d): ", MAX_VOTES);
    if (scanf("%d", &votes) != 1 || votes < 0 || votes > MAX_VOTES) {
        puts("Invalid vote count."); return 1;
    }

    for (int i = 0; i < votes; ++i) {
        int vote;
        printf("Vote %d: ", i + 1);
        if (scanf("%d", &vote) != 1) {
            puts("Invalid input format."); return 1;
        }
        if (vote >= 1 && vote <= candidates) {
            ++count[vote]; ++valid;
        } else {
            ++invalid;
        }
    }

    for (int i = 1; i <= candidates; ++i) order[i - 1] = i;

    /* Stable insertion sort by descending count; candidate ID breaks ties. */
    for (int i = 1; i < candidates; ++i) {
        int key = order[i], j = i - 1;
        while (j >= 0 &&
              (count[order[j]] < count[key] ||
              (count[order[j]] == count[key] && order[j] > key))) {
            order[j + 1] = order[j]; --j;
        }
        order[j + 1] = key;
    }

    printf("\nValid votes: %d\nInvalid votes: %d\n", valid, invalid);
    puts("Candidate ranking (candidate: votes):");
    for (int i = 0; i < candidates; ++i)
        printf("%d: %d\n", order[i], count[order[i]]);

    int winner_votes = count[order[0]], tied = 0;
    for (int i = 1; i < candidates; ++i)
        if (count[order[i]] == winner_votes) tied = 1;

    if (tied) {
        printf("Tie for highest vote count: %d votes\n", winner_votes);
        printf("Candidates tied:");
        for (int i = 0; i < candidates; ++i)
            if (count[order[i]] == winner_votes) printf(" %d", order[i]);
        puts("");
    } else {
        printf("Winner: Candidate %d with %d votes\n",
               order[0], winner_votes);
    }
    return 0;
}

