#include <stdio.h>

// -------- FCFS --------
void fcfs(int n, int bt[]) {
    int wt[n], tat[n];
    float avg_wt = 0, avg_tat = 0;

    wt[0] = 0;
    for(int i = 1; i < n; i++)
        wt[i] = wt[i-1] + bt[i-1];

    for(int i = 0; i < n; i++) {
        tat[i] = wt[i] + bt[i];
        avg_wt += wt[i];
        avg_tat += tat[i];
    }

    printf("\nProcess\tBT\tWT\tTAT\n");
    for(int i = 0; i < n; i++)
        printf("P%d\t%d\t%d\t%d\n", i+1, bt[i], wt[i], tat[i]);

    printf("Average WT = %.2f\n", avg_wt/n);
    printf("Average TAT = %.2f\n", avg_tat/n);
}

// -------- SJF --------
void sjf(int n, int bt[]) {
    int wt[n], tat[n], temp;

    // sort burst time
    for(int i = 0; i < n; i++) {
        for(int j = i+1; j < n; j++) {
            if(bt[i] > bt[j]) {
                temp = bt[i];
                bt[i] = bt[j];
                bt[j] = temp;
            }
        }
    }

    wt[0] = 0;
    for(int i = 1; i < n; i++)
        wt[i] = wt[i-1] + bt[i-1];

    for(int i = 0; i < n; i++)
        tat[i] = wt[i] + bt[i];

    printf("\nProcess\tBT\tWT\tTAT\n");
    for(int i = 0; i < n; i++)
        printf("P%d\t%d\t%d\t%d\n", i+1, bt[i], wt[i], tat[i]);
}

// -------- Round Robin --------
void roundRobin(int n, int bt[], int tq) {
    int rt[n], wt[n] = {0}, tat[n];
    int time = 0, remain = n;

    for(int i = 0; i < n; i++)
        rt[i] = bt[i];

    while(remain != 0) {
        for(int i = 0; i < n; i++) {
            if(rt[i] > 0) {
                if(rt[i] <= tq) {
                    time += rt[i];
                    wt[i] = time - bt[i];
                    rt[i] = 0;
                    remain--;
                } else {
                    time += tq;
                    rt[i] -= tq;
                }
            }
        }
    }

    for(int i = 0; i < n; i++)
        tat[i] = bt[i] + wt[i];

    printf("\nProcess\tBT\tWT\tTAT\n");
    for(int i = 0; i < n; i++)
        printf("P%d\t%d\t%d\t%d\n", i+1, bt[i], wt[i], tat[i]);
}

// -------- Main Menu --------
int main() {
    int n, choice, tq;

    printf("Enter number of processes: ");
    scanf("%d", &n);

    int bt[n];

    printf("Enter Burst Time:\n");
    for(int i = 0; i < n; i++)
        scanf("%d", &bt[i]);

    do {
        printf("\n--- CPU Scheduling ---\n");
        printf("1. FCFS\n2. SJF\n3. Round Robin\n4. Exit\n");
        printf("Enter choice: ");
        scanf("%d", &choice);

        switch(choice) {
            case 1:
                fcfs(n, bt);
                break;

            case 2:
                sjf(n, bt);
                break;

            case 3:
                printf("Enter Time Quantum: ");
                scanf("%d", &tq);
                roundRobin(n, bt, tq);
                break;

            case 4:
                printf("Exiting...\n");
                break;

            default:
                printf("Invalid choice!\n");
        }

    } while(choice != 4);

    return 0;
}
