#include <stdio.h>
#include <stdlib.h>

struct Student {
    int roll;
    char name[50];
    float m1, m2, m3, total;
};

// Add student
void addStudent() {
    FILE *fp = fopen("students.txt", "ab");
    struct Student s;

    printf("Enter Roll: ");
    scanf("%d", &s.roll);

    printf("Enter Name: ");
    scanf(" %[^\n]", s.name);

    printf("Enter 3 Marks: ");
    scanf("%f %f %f", &s.m1, &s.m2, &s.m3);

    s.total = s.m1 + s.m2 + s.m3;

    fwrite(&s, sizeof(s), 1, fp);
    fclose(fp);

    printf("Saved!\n");
}

// Display students
void displayStudents() {
    FILE *fp = fopen("students.txt", "rb");
    struct Student s;

    printf("\n--- Students ---\n");

    while (fread(&s, sizeof(s), 1, fp)) {
        printf("Roll: %d | Name: %s | Total: %.2f\n",
               s.roll, s.name, s.total);
    }

    fclose(fp);
}

// Rank list
void rankList() {
    FILE *fp = fopen("students.txt", "rb");
    struct Student s[100];
    int count = 0, i, j;

    while (fread(&s[count], sizeof(struct Student), 1, fp)) {
        count++;
    }
    fclose(fp);

    // sort
    for (i = 0; i < count - 1; i++) {
        for (j = i + 1; j < count; j++) {
            if (s[i].total < s[j].total) {
                struct Student temp = s[i];
                s[i] = s[j];
                s[j] = temp;
            }
        }
    }

    printf("\n--- Rank List ---\n");
    for (i = 0; i < count; i++) {
        printf("Rank %d: %s (%.2f)\n", i+1, s[i].name, s[i].total);
    }
}

int main() {
    int choice;

    while (1) {
        printf("\n1.Add\n2.Display\n3.Rank\n4.Exit\n");
        printf("Enter choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1: addStudent(); break;
            case 2: displayStudents(); break;
            case 3: rankList(); break;
            case 4: exit(0);
            default: printf("Wrong choice\n");
        }
    }
}
