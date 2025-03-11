# School_management_App_using_c

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_STUDENTS 100

// Define Student Structure with Address, Phone, and State
struct Student {
    char name[50];
    int roll_no;
    int marks[5];  // Marks for 5 subjects
    int total_marks;
    float percentage;
    char attendance[30];  // Attendance (e.g., "Present", "Absent")
    char address[100];  // Address field
    char phone[15];  // Phone number field
    char state[50];  // State field
};

// Global variables
struct Student students[MAX_STUDENTS];
int student_count = 0;

// Function declarations
void registerStudent();
void recordAttendance();
void recordMarks();
void searchStudent();
void displayStudentDetails(int index);
void displayAllStudents();  // Function to display all students
void displayMenu();
void saveData();
void loadData();

int main() {
    loadData();  // Load existing data from the file
    displayMenu();
    return 0;
}

// Function to display the main menu
void displayMenu() {
    int choice;
    do {
        printf("\n--- School Management System ---\n");
        printf("1. Register Student\n");
        printf("2. Record Attendance\n");
        printf("3. Record Marks\n");
        printf("4. Search Student\n");
        printf("5. Display All Students\n");  // Option to display all students
        printf("6. Save and Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);
        
        switch(choice) {
            case 1: registerStudent(); break;
            case 2: recordAttendance(); break;
            case 3: recordMarks(); break;
            case 4: searchStudent(); break;
            case 5: displayAllStudents(); break;  // Display all students
            case 6: saveData(); break;
            default: printf("Invalid choice! Try again.\n");
        }
    } while(choice != 6);
}

// Function to register a student with address, phone, and state
void registerStudent() {
    if (student_count < MAX_STUDENTS) {
        struct Student newStudent;
        printf("\nEnter student name: ");
        getchar();  // To clear newline character
        fgets(newStudent.name, 50, stdin);
        newStudent.name[strcspn(newStudent.name, "\n")] = 0;  // Remove newline
        
        printf("Enter roll number: ");
        scanf("%d", &newStudent.roll_no);
        
        printf("Enter address: ");
        getchar();  // To clear newline character after roll number input
        fgets(newStudent.address, 100, stdin);
        newStudent.address[strcspn(newStudent.address, "\n")] = 0;  // Remove newline

        printf("Enter phone number: ");
        fgets(newStudent.phone, 15, stdin);
        newStudent.phone[strcspn(newStudent.phone, "\n")] = 0;  // Remove newline

        printf("Enter state: ");
        fgets(newStudent.state, 50, stdin);
        newStudent.state[strcspn(newStudent.state, "\n")] = 0;  // Remove newline
        
        // Store the new student
        students[student_count] = newStudent;
        student_count++;
        printf("Student registered successfully!\n");
    } else {
        printf("Max student limit reached.\n");
    }
}

// Function to record attendance for a student
void recordAttendance() {
    int roll_no, i;
    printf("\nEnter roll number of student: ");
    scanf("%d", &roll_no);

    for (i = 0; i < student_count; i++) {
        if (students[i].roll_no == roll_no) {
            printf("Enter attendance (Present/Absent): ");
            getchar();  // Clear newline character
            fgets(students[i].attendance, 30, stdin);
            students[i].attendance[strcspn(students[i].attendance, "\n")] = 0;
            printf("Attendance recorded successfully.\n");
            return;
        }
    }
    printf("Student not found.\n");
}

// Function to record marks for a student
void recordMarks() {
    int roll_no, i, j;
    printf("\nEnter roll number of student: ");
    scanf("%d", &roll_no);

    for (i = 0; i < student_count; i++) {
        if (students[i].roll_no == roll_no) {
            printf("Enter marks for 5 subjects:\n");
            for (j = 0; j < 5; j++) {
                printf("Subject %d: ", j + 1);
                scanf("%d", &students[i].marks[j]);
            }

            // Calculate total marks and percentage
            students[i].total_marks = 0;
            for (j = 0; j < 5; j++) {
                students[i].total_marks += students[i].marks[j];
            }
            students[i].percentage = (float)students[i].total_marks / 5;
            printf("Marks recorded successfully.\n");
            return;
        }
    }
    printf("Student not found.\n");
}

// Function to search a student by roll number
void searchStudent() {
    int roll_no, i;
    printf("\nEnter roll number of student to search: ");
    scanf("%d", &roll_no);

    for (i = 0; i < student_count; i++) {
        if (students[i].roll_no == roll_no) {
            displayStudentDetails(i);
            return;
        }
    }
    printf("Student not found.\n");
}

// Function to display student details
void displayStudentDetails(int index) {
    printf("\nStudent Name: %s\n", students[index].name);
    printf("Roll Number: %d\n", students[index].roll_no);
    printf("Marks:\n");
    for (int i = 0; i < 5; i++) {
        printf("Subject %d: %d\n", i + 1, students[index].marks[i]);
    }
    printf("Total Marks: %d\n", students[index].total_marks);
    printf("Percentage: %.2f%%\n", students[index].percentage);
    printf("Attendance: %s\n", students[index].attendance);
    printf("Address: %s\n", students[index].address);
    printf("Phone: %s\n", students[index].phone);
    printf("State: %s\n", students[index].state);
}

// Function to display all students
void displayAllStudents() {
    if (student_count == 0) {
        printf("No students registered yet.\n");
        return;
    }
    
    printf("\n--- List of All Students ---\n");
    for (int i = 0; i < student_count; i++) {
        printf("Student %d:\n", i + 1);
        displayStudentDetails(i);
        printf("\n--------------------------------\n");
    }
}

// Function to save the student data to a file
void saveData() {
    FILE *file = fopen("school_data.dat", "wb");
    if (file != NULL) {
        fwrite(&students, sizeof(struct Student), student_count, file);
        fclose(file);
        printf("Data saved successfully.\n");
    } else {
        printf("Error saving data.\n");
    }
}

// Function to load the student data from a file
void loadData() {
    FILE *file = fopen("school_data.dat", "rb");
    if (file != NULL) {
        fread(&students, sizeof(struct Student), MAX_STUDENTS, file);
        fclose(file);
    }
}
