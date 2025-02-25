#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <arpa/inet.h>

#define PORT 8080
#define SERVER_IP "127.0.0.1"

int main() {
    int sock_fd;
    struct sockaddr_in server_addr;
    char buffer[1024];

    // Create socket
    sock_fd = socket(AF_INET, SOCK_STREAM, 0);
    if (sock_fd == -1) {
        perror("Socket creation failed");
        exit(EXIT_FAILURE);
    }

    // Connect to server
    server_addr.sin_family = AF_INET;
    server_addr.sin_port = htons(PORT);
    server_addr.sin_addr.s_addr = inet_addr(SERVER_IP);
    
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
