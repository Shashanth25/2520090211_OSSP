===============
      1Q
===============
#include <stdio.h>
#include <unistd.h>
int main()
{
    pid_t pid = fork();
    if(pid == 0)
{
    printf("Child Process\n");
    printf("PID = %d\n", getpid());
}
else
{
    printf("Parent Process\n");
    printf("PID = %d\n", getpid());
}

return 0;
}
===============
     1Q(a)
===============
#include <stdio.h>
#include <unistd.h>
int main()
{
    printf("Before exec()\n");
    execlp("date","date",NULL);
    printf("After exec()\n");
    return 0;
}


================
       2Q
================ 
#include <stdio.h>
#include <string.h>
#define MAX 100
int main()
{
    char input[MAX];
    while (1)
{
    printf("myshell> ");

    if (fgets(input, MAX, stdin) == NULL)
        break;

    input[strcspn(input, "\n")] = '\0';

    if (strcmp(input, "exit") == 0)
    {
        printf("Exiting Shell...\n");
        break;
    }

    if (strlen(input) == 0)
    {
        continue;
    }

    printf("You Entered: %s\n", input);
}

return 0;
}


================
       3Q
================ 
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#define INITIAL_SIZE 100
#define HISTORY_SIZE 10
int main()
{
    char *buffer;
    int size = INITIAL_SIZE;
    buffer = (char *)malloc(size);

if(buffer == NULL)
{
    printf("Memory Allocation Failed\n");
    return 1;
}

char *history[HISTORY_SIZE];
int count = 0;

while(1)
{
    printf("\n\033[1;32mMyShell>\033[0m ");

    if(fgets(buffer, size, stdin) == NULL)
        break;

    buffer[strcspn(buffer,"\n")] = '\0';

    if(strcmp(buffer,"exit") == 0)
        break;

    if(strcmp(buffer,"history") == 0)
    {
        printf("\nCommand History:\n");

        for(int i=0;i<count;i++)
        {
            printf("%d : %s\n",i+1,history[i]);
        }

        continue;
    }

    if(count < HISTORY_SIZE)
    {
        history[count] = strdup(buffer);
        count++;
    }

    if(strlen(buffer) > size-10)
    {
        size *= 2;
        buffer = realloc(buffer,size);

        if(buffer==NULL)
        {
            printf("Memory Reallocation Failed\n");
            return 1;
        }
    }

    printf("Command Entered: %s\n",buffer);
}

for(int i=0;i<count;i++)
    free(history[i]);

free(buffer);

printf("Memory Released Successfully.\n");

return 0;
}


================
       4Q
================ 
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#define MAX_INPUT 100
#define MAX_TOKENS 20
int main()
{
    char input[MAX_INPUT];
    char *tokens[MAX_TOKENS];
    int count;
    while (1)
{
    printf("MyShell> ");

    if (fgets(input, sizeof(input), stdin) == NULL)
        break;

    input[strcspn(input, "\n")] = '\0';

    if (strlen(input) == 0)
    {
        printf("Empty command!\n");
        continue;
    }

    if (strcmp(input, "exit") == 0)
        break;

    count = 0;

    char *token = strtok(input, " \t");

    while (token != NULL && count < MAX_TOKENS)
    {
        tokens[count++] = token;
        token = strtok(NULL, " \t");
    }

    printf("\nTokens:\n");

    for (int i = 0; i < count; i++)
    {
        printf("Token %d : %s\n", i + 1, tokens[i]);
    }

    printf("\nParse Tree:\n");
    printf("Command\n");

    for (int i = 0; i < count; i++)
    {
        printf(" └── %s\n", tokens[i]);
    }

    printf("\nSyntax Valid.\n\n");
}

printf("Shell Closed.\n");

return 0;
}


================
       5Q
================ 
#!/bin/bash
echo "= SKILL 5: QUOTING IN SHELL ="
name="Shashanth"
echo
echo "--- Single Quotes ---"
echo 'Hello $name'
echo 'Single quotes preserve literal content'
echo 'Spaces are preserved'
echo 'Special characters: $ @ # *'
echo
echo "--- Double Quotes ---"
echo "Hello $name"
echo "Double quotes allow variable expansion"
echo "Spaces are preserved in double quotes"
echo
echo "--- Comparing Single and Double Quotes ---"
echo 'Name is $name'
echo "Name is $name"
echo
echo "--- Quoted Command ---"
text="Linux Operating System"
echo "Text: $text"
echo
echo "--- Edge Case ---"
echo 'This is a $variable'
echo "This is a $name"
echo
echo "= SKILL 5 COMPLETED ="


================
       6Q(a)
================ 
#include <stdio.h>
#include <string.h>
int main()
{
    char input[200];
    printf("===== SKILL 6: ESCAPE SEQUENCES =====\n");

printf("Enter a string: ");
fgets(input, sizeof(input), stdin);

printf("\nOriginal Input:\n");
printf("%s", input);

printf("\nProcessed Input:\n");

for (int i = 0; i < strlen(input); i++)
{
    if (input[i] == '\\' && input[i + 1] != '\0')
    {
        i++;
        printf("%c", input[i]);
    }
    else
    {
        printf("%c", input[i]);
    }
}

printf("\n");

return 0;
}



================
       6Q(b)
================ 
#include <stdio.h>
#include <unistd.h>
#include <sys/wait.h>
#include <stdlib.h>
int main()
{
    pid_t pid;
    printf("===== SKILL 6: PROCESS CREATION =====\n");

printf("Parent process started.\n");
printf("Parent PID: %d\n", getpid());

pid = fork();

if (pid < 0)
{
    perror("fork failed");
    return 1;
}

if (pid == 0)
{
    printf("\nChild process created.\n");
    printf("Child PID: %d\n", getpid());

    printf("Child is executing: ls -l\n\n");

    execlp("ls", "ls", "-l", NULL);

    perror("exec failed");
    exit(1);
}
else
{
    printf("Parent is waiting for child...\n");

    wait(NULL);

    printf("\nChild process completed.\n");
    printf("Parent process completed.\n");
}

return 0;
}


================
      7Q(a)
================ 
#include <stdio.h>
#include <unistd.h>
#include <sys/wait.h>
#include <stdlib.h>
int main()
{
    pid_t pid;
    int status;
    printf("===== SKILL 7: waitpid() =====\n");

pid = fork();

if (pid < 0)
{
    perror("fork failed");
    return 1;
}

if (pid == 0)
{
    printf("Child process started.\n");
    printf("Child PID: %d\n", getpid());

    sleep(3);

    printf("Child process finished.\n");
    exit(0);
}
else
{
    printf("Parent process started.\n");
    printf("Parent is waiting for child using waitpid()...\n");

    waitpid(pid, &status, 0);

    if (WIFEXITED(status))
    {
        printf("Child exited normally.\n");
        printf("Child exit status: %d\n", WEXITSTATUS(status));
    }

    printf("Parent process completed.\n");
}

return 0;
}


================
      7Q(b)
================ 
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
int main()
{
    char *path;
    char *path_copy;
    char *dir;
    char fullpath[1024];
    printf("===== SKILL 7: PATH RESOLUTION =====\n");

path = getenv("PATH");

if (path == NULL)
{
    printf("PATH variable not found.\n");
    return 1;
}

printf("PATH variable:\n%s\n\n", path);

path_copy = strdup(path);

if (path_copy == NULL)
{
    perror("strdup failed");
    return 1;
}

char command[100];

printf("Enter command to search: ");
scanf("%99s", command);

dir = strtok(path_copy, ":");

while (dir != NULL)
{
    snprintf(fullpath, sizeof(fullpath), "%s/%s", dir, command);

    if (access(fullpath, X_OK) == 0)
    {
        printf("Command found: %s\n", fullpath);
        free(path_copy);
        return 0;
    }

    dir = strtok(NULL, ":");
}

printf("Command not found: %s\n", command);

free(path_copy);

return 0;
}


================
      8Q(a)
================ 
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
void expand_variable(char *input)
{
    char variable[100];
    char result[500] = "";
    char *start;
    char *end;
    start = strchr(input, '$');

if (start == NULL)
{
    printf("No variable reference found.\n");
    return;
}

end = start + 1;

while (*end != '\0' &&
       ((*end >= 'A' && *end <= 'Z') ||
        (*end >= 'a' && *end <= 'z') ||
        (*end >= '0' && *end <= '9') ||
        *end == '_'))
{
    end++;
}

int length = end - start - 1;

if (length == 0)
{
    printf("Invalid variable reference.\n");
    return;
}

strncpy(variable, start + 1, length);
variable[length] = '\0';

char *value = getenv(variable);

if (value == NULL)
{
    printf("Undefined variable: %s\n", variable);
    return;
}

strncpy(result, input, start - input);
result[start - input] = '\0';

strcat(result, value);
strcat(result, end);

printf("Expanded result: %s\n", result);
}
int main()
{
    char input[500];
    printf("===== SKILL 8: VARIABLE EXPANSION =====\n");

printf("Enter text containing a variable: ");
fgets(input, sizeof(input), stdin);

input[strcspn(input, "\n")] = '\0';

printf("Original: %s\n", input);

expand_variable(input);

return 0;
}


================
      8Q(b)
================ 
#include <stdio.h>
#include <string.h>
#include <unistd.h>
#include <stdlib.h>
void builtin_cd(char *args)
{
    if (args == NULL)
    {
        printf("cd: missing directory\n");
        return;
    }
    if (chdir(args) != 0)
    {
    perror("cd");
    }
}
void builtin_pwd()
{
    char cwd[500];
    if (getcwd(cwd, sizeof(cwd)) != NULL)
    printf("%s\n", cwd);
else
    perror("pwd");
}
void builtin_help()
{
    printf("Built-in commands:\n");
    printf("cd    - change directory\n");
    printf("pwd   - print current directory\n");
    printf("help  - display help\n");
    printf("exit  - exit shell\n");
}
int main()
{
    char command[100];
    char argument[200];
    printf("===== SKILL 8: BUILT-IN COMMANDS =====\n");

while (1)
{
    printf("skill8$ ");

    if (scanf("%99s", command) != 1)
        break;

    if (strcmp(command, "exit") == 0)
    {
        printf("Exiting shell...\n");
        break;
    }
    else if (strcmp(command, "pwd") == 0)
    {
        builtin_pwd();
    }
    else if (strcmp(command, "help") == 0)
    {
        builtin_help();
    }
    else if (strcmp(command, "cd") == 0)
    {
        scanf("%199s", argument);
        builtin_cd(argument);
    }
    else
    {
        printf("Invalid command: %s\n", command);
    }
}

return 0;
}
