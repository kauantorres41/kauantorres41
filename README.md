- 👋 Hi, I’m @kauantorres41
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
kauantorres41/kauantorres41 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
#include <stdio.h>
#include <string.h>

#define DISK_SIZE 20
#define MAX_NAME_LEN 30

typedef struct {
    char name[MAX_NAME_LEN];
    int occupied;
} File;

void initializeDisk(File disk[]) {
    for (int i = 0; i < DISK_SIZE; i++) {
        disk[i].occupied = 0;
        strcpy(disk[i].name, "EMPTY");
    }
}

void displayDisk(File disk[]) {
    printf("Estado do disco: ");
    for (int i = 0; i < DISK_SIZE; i++) {
        printf("[%s] ", disk[i].name);
    }
    printf("\n");
}

void insertFile(File disk[], int position, const char* filename) {
    if (position < 0 || position >= DISK_SIZE) {
        printf("Posição inválida.\n");
        return;
    }

    // Verifica se há fragmentação e se o arquivo pode ser inserido
    if (disk[position].occupied) {
        printf("Espaço já ocupado.\n");
    } else {
        strcpy(disk[position].name, filename);
        disk[position].occupied = 1;
        printf("Arquivo '%s' inserido na posição %d.\n", filename, position);
    }
}

void removeFile(File disk[], int position) {
    if (position < 0 || position >= DISK_SIZE) {
        printf("Posição inválida.\n");
        return;
    }

    if (disk[position].occupied) {
        strcpy(disk[position].name, "EMPTY");
        disk[position].occupied = 0;
        printf("Arquivo removido da posição %d.\n", position);
    } else {
        printf("Espaço já está vazio.\n");
    }
}

void defragmentDisk(File disk[]) {
    int nextFree = 0;

    for (int i = 0; i < DISK_SIZE; i++) {
        if (disk[i].occupied) {
            if (i != nextFree) {
                // Mover temporariamente o arquivo para um espaço vazio
                printf("Movendo temporariamente '%s' da posição %d para espaço vazio...\n", disk[i].name, i);
                displayDisk(disk);
                getchar();

                strcpy(disk[nextFree].name, disk[i].name);
                disk[nextFree].occupied = 1;

                strcpy(disk[i].name, "EMPTY");
                disk[i].occupied = 0;
                printf("Arquivo '%s' movido para posição %d.\n", disk[nextFree].name, nextFree);
                displayDisk(disk);
                getchar();
            }
            nextFree++;
        }
    }
    printf("Disco desfragmentado.\n");
}

void sortDisk(File disk[]) {
    // Organizar arquivos movendo-os para os primeiros espaços vazios contíguos
    for (int i = 0; i < DISK_SIZE - 1; i++) {
        for (int j = i + 1; j < DISK_SIZE; j++) {
            if (disk[i].occupied && disk[j].occupied && strcmp(disk[i].name, disk[j].name) > 0) {
                // Trocar arquivos
                File temp;
                strcpy(temp.name, disk[i].name);
                temp.occupied = disk[i].occupied;

                strcpy(disk[i].name, "EMPTY");
                disk[i].occupied = 0;

                printf("Movendo '%s' para espaço vazio temporariamente...\n", temp.name);
                displayDisk(disk);
                getchar();

                strcpy(disk[i].name, disk[j].name);
                disk[i].occupied = disk[j].occupied;

                strcpy(disk[j].name, temp.name);
                disk[j].occupied = temp.occupied;

                printf("Arquivo '%s' movido para posição %d.\n", disk[i].name, i);
                displayDisk(disk);
                getchar();
            }
        }
    }
    printf("Arquivos organizados.\n");
}

void mergeFragments(File disk[]) {
    for (int i = 0; i < DISK_SIZE; i++) {
        if (disk[i].occupied) {
            int j = i + 1;
            while (j < DISK_SIZE && strcmp(disk[j].name, disk[i].name) == 0) {
                if (disk[j].occupied) {
                    int k = i;
                    // Mover para a posição disponível mais próxima
                    while (k < DISK_SIZE && disk[k].occupied) k++;
                    if (k < DISK_SIZE) {
                        strcpy(disk[k].name, disk[j].name);
                        disk[k].occupied = 1;
                        strcpy(disk[j].name, "EMPTY");
                        disk[j].occupied = 0;
                        printf("Arquivo '%s' movido para a posição %d.\n", disk[k].name, k);
                        displayDisk(disk);
                        getchar();
                    }
                }
                j++;
            }
        }
    }
    printf("Fragmentos mesclados.\n");
}

int main() {
    File disk[DISK_SIZE];
    int choice, position;
    char filename[MAX_NAME_LEN];

    initializeDisk(disk);

    while (1) {
        printf("\n1. Exibir disco\n2. Inserir arquivo\n3. Remover arquivo\n4. Organizar arquivos\n5. Desfragmentar disco\n6. Mesclar fragmentos\n7. Sair\nEscolha uma opção: ");
        scanf("%d", &choice);
        getchar();

        switch (choice) {
            case 1:
                displayDisk(disk);
                break;
            case 2:
                printf("Digite o nome do arquivo: ");
                scanf("%s", filename);
                printf("Digite a posição para inserir (0 a %d): ", DISK_SIZE - 1);
                scanf("%d", &position);
                insertFile(disk, position, filename);
                break;
            case 3:
                printf("Digite a posição para remover (0 a %d): ", DISK_SIZE - 1);
                scanf("%d", &position);
                removeFile(disk, position);
                break;
            case 4:
                sortDisk(disk);
                displayDisk(disk);
                break;
            case 5:
                defragmentDisk(disk);
                displayDisk(disk);
                break;
            case 6:
                mergeFragments(disk);
                displayDisk(disk);
                break;
            case 7:
                printf("Saindo...\n");
                return 0;
            default:
                printf("Opção inválida.\n");
        }
    }

    return 0;
}
ajusta o código para organizar o disco de forma correta 
