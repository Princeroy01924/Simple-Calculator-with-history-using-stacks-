#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX 100

typedef struct {
    char history[MAX][50];
    int top;
} Stack;

void push(Stack *s, const char *entry) {
    if (s->top < MAX - 1) {
        s->top++;
        strcpy(s->history[s->top], entry);
    } else {
        printf("History stack full!\n");
    }
}

void displayHistory(Stack *s) {
    if (s->top == -1) {
        printf("No history yet.\n");
        return;
    }
    printf("\n--- Calculation History ---\n");
    for (int i = s->top; i >= 0; i--) {
        printf("%s\n", s->history[i]);
    }
    printf("---------------------------\n");
}

int main() {
    Stack historyStack;
    historyStack.top = -1;

    int choice;
    double a, b, result;
    char op[2], entry[50];

    while (1) {
        printf("\nSimple Calculator with History\n");
        printf("1. Perform Calculation\n");
        printf("2. View History\n");
        printf("3. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        if (choice == 1) {
            printf("Enter expression (e.g., 5 + 3): ");
            scanf("%lf %s %lf", &a, op, &b);

            if (strcmp(op, "+") == 0) {
                result = a + b;
            } else if (strcmp(op, "-") == 0) {
                result = a - b;
            } else if (strcmp(op, "*") == 0) {
                result = a * b;
            } else if (strcmp(op, "/") == 0) {
                if (b != 0)
                    result = a / b;
                else {
                    printf("Error: Division by zero.\n");
                    continue;
                }
            } else {
                printf("Invalid operator.\n");
                continue;
            }

            printf("Result: %.2lf\n", result);
            sprintf(entry, "%.2lf %s %.2lf = %.2lf", a, op, b, result);
            push(&historyStack, entry);

        } else if (choice == 2) {
            displayHistory(&historyStack);
        } else if (choice == 3) {
            printf("Exiting...\n");
            break;
        } else {
            printf("Invalid choice. Try again.\n");
        }
    }

    return 0;
}
