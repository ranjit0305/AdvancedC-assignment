# Advance C Programming Assignment Solutions

---

# Topic 1: Structures

## Weekly Calendar Program

```c
#include <stdio.h>

#define DAYS 7
#define MAX_TASKS 3

struct Day {
    char dayName[20];
    char tasks[MAX_TASKS][50];
    int taskCount;
};

int main() {

    struct Day week[DAYS] = {
        {"Monday"},
        {"Tuesday"},
        {"Wednesday"},
        {"Thursday"},
        {"Friday"},
        {"Saturday"},
        {"Sunday"}
    };

    int i, j;

    // Input tasks
    for(i = 0; i < DAYS; i++) {

        printf("\nEnter number of tasks for %s (max 3): ",
               week[i].dayName);

        scanf("%d", &week[i].taskCount);

        if(week[i].taskCount > 3)
            week[i].taskCount = 3;

        for(j = 0; j < week[i].taskCount; j++) {

            printf("Enter task %d: ", j + 1);

            scanf("%s", week[i].tasks[j]);
        }
    }

    // Display tasks
    printf("\n===== WEEKLY TASKS =====\n");

    for(i = 0; i < DAYS; i++) {

        printf("\n%s:\n", week[i].dayName);

        for(j = 0; j < week[i].taskCount; j++) {

            printf("%d. %s\n",j + 1,week[i].tasks[j]);
        }
    }

    return 0;
}
```

---

# Topic 2: Pointers

## Rearrange Even Numbers Before Odd Numbers

```c
#include <stdio.h>

void rearrange(int *arr, int size) {

    int *i, *j;
    int temp;

    for(i = arr; i < arr + size; i++) {

        if(*i % 2 != 0) {

            for(j = i + 1; j < arr + size; j++) {

                if(*j % 2 == 0) {

                    temp = *j;

                    int *k;

                    for(k = j; k > i; k--) {
                        *k = *(k - 1);
                    }

                    *i = temp;

                    break;
                }
            }
        }
    }
}

int main() {

    int arr[] = {1, 2, 3, 4, 5, 6, 7};

    int size = sizeof(arr) / sizeof(arr[0]);

    rearrange(arr, size);

    printf("Rearranged Array:\n");

    int *p;

    for(p = arr; p < arr + size; p++) {
        printf("%d ", *p);
    }

    return 0;
}
```

### Output

```text
2 4 6 1 3 5 7
```

---

# Topic 3: Arrays

## Search Element in Sorted Matrix

```c
#include <stdio.h>

int searchMatrix(int arr[][100], int n, int key) {

    int row = 0;
    int col = n - 1;

    while(row < n && col >= 0) {

        if(arr[row][col] == key) {
            return 1;
        }

        else if(arr[row][col] > key) {
            col--;
        }

        else {
            row++;
        }
    }

    return 0;
}

int main() {

    int n, key;
    int arr[100][100];

    printf("Enter size of matrix: ");
    scanf("%d", &n);

    printf("Enter matrix elements:\n");

    for(int i = 0; i < n; i++) {

        for(int j = 0; j < n; j++) {
            scanf("%d", &arr[i][j]);
        }
    }

    printf("Enter key to search: ");
    scanf("%d", &key);

    if(searchMatrix(arr, n, key))
        printf("Key Found");
    else
        printf("Key Not Found");

    return 0;
}
```

### Example Matrix

```text
1   4   7   11
2   5   8   12
3   6   9   16
10  13  14  17
```

---