#include <stdio.h>
#include <stdlib.h>
#include <sys/socket.h>
#include <sys/un.h>
#include <unistd.h>

#define SOCKET_PATH "/tmp/unix_socket"

int main() {
    int sock_fd;
    struct sockaddr_un server_addr;
    char buffer[1024];

    // Create socket
    sock_fd = socket(AF_UNIX, SOCK_STREAM, 0);
    if (sock_fd == -1) {
        perror("Socket creation failed");
        exit(EXIT_FAILURE);
    }

    // Connect to server
    server_addr.sun_family = AF_UNIX;
    strcpy(server_addr.sun_path, SOCKET_PATH);
    if (connect(sock_fd, (struct sockaddr *)&server_addr, sizeof(server_addr)) == -1) {
        perror("Connection failed");
        exit(EXIT_FAILURE);
    }

    printf("Connected to server. Start chatting!\n");
    while (1) {
        printf("You: ");
        fgets(buffer, sizeof(buffer), stdin);
        send(sock_fd, buffer, strlen(buffer), 0);
        int bytes = recv(sock_fd, buffer, sizeof(buffer) - 1, 0);
        if (bytes <= 0) {
            printf("Server disconnected.\n");
            break;
        }
        buffer[bytes] = '\0';
        printf("Server: %s", buffer);
    }

    close(sock_fd);
    return 0;
}
