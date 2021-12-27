#include<stdio.h>        //lib de input output
#include<stdlib.h>       //lib de funções gerais
#include<string.h>       //lib para manipular strings
#include<sys/types.h>    // AF_INET, SOCK_STREAM
#include<sys/socket.h>   // socket(), connect()
#include<arpa/inet.h>    //lib para operação na internet
#include<unistd.h>       // close()
#include<stdbool.h>

short CreateSocket(void){
    short hSocket;
    printf("creating socket ...\n");
    hSocket = socket(AF_INET, SOCK_STREAM, 0); //ref: https://pubs.opengroup.org/onlinepubs/7908799/xns/socket.html
    return hSocket;
}

bool IsSocketError(short hSocket){
    if(hSocket == -1){
        return true;
    }
    return false;
}

bool IsConnectionError(short iRetval){
    if(iRetval == -1){
        return true;
    }
    return false;
}

int CreateSocketConnection(short hSocket){
    int iRetval = -1;
    int port = 8080;
    struct sockaddr_in remote = {0}; //ref: https://www.ibm.com/docs/en/ztpf/1.1.0.15?topic=functions-structures-defined-in-syssocketh-header-file
    remote.sin_addr.s_addr = inet_addr("127.0.0.1"); //ref: https://pubs.opengroup.org/onlinepubs/7908799/xns/inet_addr.html
    remote.sin_family = AF_INET;
    remote.sin_port = htons(port);
    // (struct sockaddr *)&remote -> atribui o endereço de memória do objeto remote para o ponteiro da classe sockaddr
    // sizeof(struct sockaddr_in) -> retorna a quantidade de bytes que a classe sockadrr_in usa
    iRetval = connect(hSocket, (struct sockaddr *)&remote, sizeof(struct sockaddr_in));
    return iRetval;
}

int SocketSend(int hSocket, char* message, short lenMessage){
    int opt = -1;
    int shortRetval = -1;

    struct timeval tv; // Classe para formatar configuração de timeout do socket
    tv.tv_sec = 20;
    tv.tv_usec = 0;
    shortRetval = send(hSocket, message, lenMessage, 0); //ref: https://pubs.opengroup.org/onlinepubs/7908799/xns/send.html
    return shortRetval;
}

int SocketReceive(int hSocket, char* response, short lenMessage){
    int opt = -1;
    int shortRetval = -1;

    struct timeval tv; // Classe para formatar configuração de timeout do socket
    tv.tv_sec = 20;
    tv.tv_usec = 0;
    shortRetval = recv(hSocket, response, lenMessage, 0); //ref: https://pubs.opengroup.org/onlinepubs/7908799/xns/recv.html
    printf("Response %s\n",response);
    return shortRetval;
}

int main(int argc, char *argv[]){

    int iRetval, read_size = -1;
    char message[100] = {0};
    char server_reply[200] = {0};
    int hSocket = -1;

    hSocket = CreateSocket();
    if(IsSocketError(hSocket)){
        printf("Could not create socket :( \n");
        return 1;
    }
    printf("socket is created ...\n]");

    iRetval = CreateSocketConnection(hSocket);
    if(IsConnectionError(iRetval)){
        printf("Could not connect socket :( \n");
        return 1;
    }
    printf("Connection started with server ..\n");

    printf("Enter the Message: ");
    fgets(message, 100, stdin); //função para pegar input do console

    SocketSend(hSocket, message, strlen(message));
    read_size = SocketReceive(hSocket, server_reply, 200);

    printf("Server Response : %s\n\n",server_reply);

    close(hSocket);
    shutdown(hSocket,0);
    shutdown(hSocket,1);
    shutdown(hSocket,2);
    return 0;
}
