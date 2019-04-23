# san_c Project in C
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
struct student
{
    char htno[10], name[50];
};
struct instructor
{
	char Id[10], name[50];
};
struct course
{
	char Id[10], name[50];
};

struct student students[100];  int nos=0;
struct instructor instructors[100];  int noi = 0;
struct course courses[100]; int noc = 0;

char fName[4][20] = {"", "students.txt", "instructors.txt", "courses.txt"};
char optionNames[4][20] = {"", "STUDENT", "INSTRUCTOR", "COURSE"};

int option; // 1-student  2-instructor  3-course

FILE *filename;

void menu1();
void menu2();

void createDummy(char *fname)
{
	FILE *fp;
	char line[100], tmp1[10], tmp2[50];
	int i;
	fp = fopen(fname, "r");
	i=0;
	while(!feof(fp)){
		fgets(line, 100, fp);
		if( notEmptyLine(line) )
		{
			sscanf(line, "%s %s", tmp1, tmp2);
			if(option==1) {
				strcpy(students[nos].htno, tmp1);
				strcpy(students[nos].name, tmp2);
				printf("%s %s\n", students[nos].htno, students[nos].name);
				nos++;
			}
			else if(option==2) {
				strcpy(instructors[noi].Id, tmp1);
				strcpy(instructors[noi].name, tmp2);
				printf("%s %s\n", instructors[noi].Id, instructors[noi].name);
				noi++;
			}
			else if(option==3) {
				strcpy(courses[noc].Id, tmp1);
				strcpy(courses[noc].name, tmp2);
				printf("%s %s\n", courses[noc].Id, courses[noc].name);
				noc++;
			}
			strcpy(line, "");
			i++;
		}
	}
	puts("ok");
	fclose(fp);
}

void delete_Record(char *fname)
{
	FILE *fp;
	char htno[10];
	int i, duplicatefound;
	
	createDummy(fname);
	
	if(option==1 && nos==0) {printf("No Records found in %s\n", fName[option]);return ;}
	if(option==2 && noi==0) {printf("No Records found in %s\n", fName[option]);return ;}
	if(option==3 && noc==0) {printf("No Records found in %s\n", fName[option]);return ;}
	switch (option)
	{
		case 1:
			printf("Enter Hallticket Number");
			scanf("%s", htno);
			fp = fopen(fname, "w");
			for(i=0; i<nos; i++)
			{
				if( strcmp(students[i].htno, htno)==0 )
					continue;
				else 
					fprintf(fp, "%s %s\n", students[i].htno, students[i].name);
			}
			puts("Record deleted successfullt.");
			fclose(fp);
			break;
	}
	// fp = fopen(fname, "ab");
	// printf("* Delete Record from %s *\n", optionNames[option] );
	// puts("------------------------");
	// switch(option)
	// {
		// case 1: 	// Student details
			// printf("Enter Hallticket Number");
			// scanf("%s", htno);
			// break;
			
		// case 2: 	// Instructor details
			// printf("Enter Faculty Id");
			// scanf("%s", htno);
			// break;
			
		// case 3: 	// Course details
			// printf("Enter Course Id");
			// scanf("%s", htno);
			// break;
	// }
	// duplicatefound = checkforDuplicate(fname, htno);
	// if(duplicatefound == 1)
	// {
		// printf("Record Deleted...");
	// }
	// else 
	// {
		// if(duplicatefound == -1)
			// printf("No records were found in %s", fName[option]);
	// }
	// fclose(fp);	
}
int notEmptyLine(char *line)
{
	int i=0, empty=0;
	for(i=0; i<strlen(line); i++)
		if(line[i]!=' ')  // check for all empty characters
			empty++;
	if(empty == strlen(line))
		return 0;
	else
		return 1;
}
void display_All(char *fname)
{
	FILE *fp;
	char line[100], tmp1[10], tmp2[50];
	int i;
	fp = fopen(fname, "rb");
	i=0;
	printf("**  %s  Details  **\n", optionNames[option] );
	puts("==========================");
	if(option==1) printf("%s %s", "HTNO ", "Student Name");
	else if(option==2) printf("%s %s", "Faculty Id.", "Faculty Name");
	else if(option==3) printf("%s %s", "Course Id.", "Course Name");
	puts("\n------------------------");
	while(!feof(fp)){
		fgets(line, 100, fp);
		if( notEmptyLine(line) )
		{
			printf("%s", line);
			strcpy(line, "");
			i++;
		}
	}
	if(i==0) puts(" No Records Available ");
	puts("------------------------");
	fclose(fp);
}

void display_Given(char *fname)
{
	FILE *fp;
	char line[100], search[10], tempId[10];
	int i, NA=0;
	fp = fopen(fname, "rb");
	i=0;
	printf("* Search %s  Record *\n", optionNames[option] );
	printf("Enter %s Id >> ", optionNames[option]);
	scanf("%s", search);
	puts("==========================");
	if(option==1) printf("%s %s", "HTNO  ", "Student Name");
	else if(option==2) printf("%s %s", "Faculty Id.", "Faculty Name");
	else if(option==3) printf("%s %s", "Course Id.", " Course Name");
	puts("\n------------------------");
	while(!feof(fp)){
		fgets(line, 100, fp);
		if(strlen(line)>0)
		{
			sscanf(line, "%s", tempId);
			if ( strcmp(search, tempId)==0)
			{
				printf("%s", line);
				NA=1;
			}
			strcpy(line, "");
			i++;
		}
	}
	if(i==0 || NA==0) puts(" No Record is Available ");
	puts("------------------------");
	fclose(fp);
}
int checkforDuplicate(char *fname, char *search)
{
	FILE *fp;
	char line[100], tempId[10];
	int i=0;
	
	fp = fopen(fname, "rb");
	
	while(!feof(fp)){
		fgets(line, 100, fp);
		if(strlen(line)>0)
		{
			sscanf(line, "%s", tempId);
			if ( strcmp(search, tempId)==0)
				return 1;	// record is matched
			i++;
		}
	}
	if(i==0) return -1;	// no records found
	else return 0;		// no record matched
}
void add_New(char *fname)
{
	FILE *fp;
	char htno[10], name[50];
	int duplicatefound;
	
	fp = fopen(fname, "ab");
	printf("* Add New Record for %s *\n", optionNames[option] );
	puts("------------------------");
	switch(option)
	{
		case 1: 	// Student details
			printf("Enter Hallticket Number");
			scanf("%s", htno);
			break;
			
		case 2: 	// Instructor details
			printf("Enter Faculty Id");
			scanf("%s", htno);
			break;
			
		case 3: 	// Course details
			printf("Enter Course Id");
			scanf("%s", htno);
			break;
	}
	duplicatefound = checkforDuplicate(fname, htno);
	if(duplicatefound == 1)
	{
		printf("\nSorry!! Duplicate record (%s) found in %s ", htno, fName[option]);
		printf("\nNote:- All alphabets are 'CASE SENSITIVE'");
	}
	else 
	{
		if(duplicatefound == -1)
			printf("No records were found in %s", fName[option]);
		
		printf("\n\nEnter Name of the %s (without giving space)>> ", optionNames[option]);
		scanf("%s", name);
		fprintf(fp, "%s %s\n", htno, name);
		puts("---Record Added successfully---");
	}
	fclose(fp);
}

void menu1()
{
	char ch1;
	while(1)
	{
		puts("			---- MAIN MENU ------");
		puts("			1. Student Options");
		puts("			2. Instructior Options");
		puts("			3. Course Options");
		puts("			Any other to quit");
		printf("			Enter you choice.. 1 / 2 / 3 >>");
		scanf("%c", &ch1);
		switch(ch1)
		{
			case '1' :
				option = 1;
				menu2();
				break;
			case '2' :
				option = 2;
				menu2();
				break;
			case '3' :
				option = 3;
				menu2();
				break;
			default: 
				puts("Good Bye...");
				return;
		}
		fflush(stdin);
	}
}

void menu2()
{
	int ch2;
	while(1)
	{
		printf("\n		--%s  MENU --\n", optionNames[option]);
		puts("		1. Display All");
		puts("		2. Display details of given ID ");
		puts("		3. Add New Record");
		puts("		4. Delete Record");
		puts("		5. Modify Record");
		puts("		Any other to go Main Menu");
		puts("		6. QUIT the application");
		printf("		Enter you choice.. 1/2/3/4/ /6 >> ");
		scanf("%d", &ch2);
		switch(ch2)
		{
			case 1 :
				display_All(fName[option]);
				break;
				
			case 2 :
				display_Given(fName[option]);
				break;
			
			case 3 :
				add_New(fName[option]);
				break;
				
			case 4 :
				delete_Record(fName[option]);
				break;
						
			case 6 :
				printf("Good Bye...");
				exit(0);
				
			default: 
				puts("Exit from Menu-2");
				return;
		}
		fflush(stdin);
	}
}

int main()
{
	menu1();
}
