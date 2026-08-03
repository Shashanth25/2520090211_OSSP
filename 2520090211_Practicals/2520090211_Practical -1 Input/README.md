1Q)
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>
int main()
{
    char command[100];
printf("Enter Linux Command: ");
scanf("%s", command);

pid_t pid = fork();

if (pid < 0)
{
    printf("Fork Failed!\n");
    return 1;
}

else if (pid == 0)
{
    printf("\nChild Process\n");
    printf("Child PID = %d\n", getpid());

    execlp(command, command, NULL);

    printf("Command execution failed.\n");
    exit(1);
}

else
{
    printf("\nParent Process\n");
    printf("Parent PID = %d\n", getpid());

    wait(NULL);

    printf("Child process completed.\n");
}

return 0;

2Q)
#include <stdio.h>
#include <fcntl.h>
#include <unistd.h>
#include <stdlib.h>
int main()
{
    int source, destination;
    char buffer[1024];
    ssize_t bytesRead;
    source = open("source.txt", O_RDONLY);

if (source < 0)
{
    printf("Cannot open source file.\n");
    return 1;
}

destination = open("destination.txt",
                   O_WRONLY | O_CREAT | O_TRUNC,
                   0644);

if (destination < 0)
{
    printf("Cannot create destination file.\n");
    close(source);
    return 1;
}

while ((bytesRead = read(source, buffer, sizeof(buffer))) > 0)
{
    write(destination, buffer, bytesRead);
}

close(source);
close(destination);

printf("File copied successfully.\n");

return 0;

3Q)
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>
#include <stdlib.h>
int main()
{
    pid_t pid;
    pid = fork();

if (pid < 0)
{
    printf("Fork failed!\n");
    return 1;
}

else if (pid == 0)
{
    printf("\n----- Child Process -----\n");
    printf("Child PID  : %d\n", getpid());
    printf("Parent PID : %d\n", getppid());

    printf("Child is running...\n");
    sleep(10);

    printf("Child process exiting...\n");
    exit(0);
}

else
{
    printf("\n----- Parent Process -----\n");
    printf("Parent PID : %d\n", getpid());
    printf("Child PID  : %d\n", pid);

    printf("Parent waiting for child...\n");
    wait(NULL);

    printf("Child terminated.\n");
}

return 0;
