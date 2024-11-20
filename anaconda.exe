#include<iostream>
#include<conio.h>
#include<stdlib.h>
#include<windows.h>

#define ARRIBA 72
#define IZQUIERDA 75
#define DERECHA 77
#define ABAJO 80
#define ESC 27
#define NUM_COMIDA 5  // Número de cuadros de comida

int cuerpo[200][2];
int n = 1;
int tam = 10;
int x = 10, y = 12;
int dir = 3;
int velocidad = 100, h = 1;
int puntuacion = 0;
int vidas = 3; // Número de vidas

char tecla;

// Arreglos para almacenar posiciones de los cuadros de comida
int xc[NUM_COMIDA];
int yc[NUM_COMIDA];

void gotoxy(int x, int y) {
    HANDLE hCon;
    COORD dwPos;
    dwPos.X = x;
    dwPos.Y = y;
    hCon = GetStdHandle(STD_OUTPUT_HANDLE);
    SetConsoleCursorPosition(hCon, dwPos);
}

void setColor(int color) {
    SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE), color);
}

void pintar() {
    setColor(10); // Verde para las paredes
    for (int i = 2; i < 78; i++) {
        gotoxy(i, 3); printf("%c", 205);
        gotoxy(i, 23); printf("%c", 205);
    }
    for (int i = 4; i < 23; i++) {
        gotoxy(2, i); printf("%c", 186);
        gotoxy(77, i); printf("%c", 186);
    }
    gotoxy(2, 3); printf("%c", 201);
    gotoxy(2, 23); printf("%c", 200);
    gotoxy(77, 3); printf("%c", 187);
    gotoxy(77, 23); printf("%c", 188);
    setColor(7); // Regresar a color blanco
}

void inicializar_comida() {
    // Genera posiciones aleatorias para cada cuadro de comida
    setColor(14); // Amarillo para la comida
    for (int i = 0; i < NUM_COMIDA; i++) {
        xc[i] = (rand() % 73) + 4;
        yc[i] = (rand() % 19) + 4;
        gotoxy(xc[i], yc[i]); printf("%c", 207);
    }
    setColor(7); // Regresar a color blanco
}

void limpiar_area_juego() {
    // Borra el área de juego para eliminar cualquier rastro de la serpiente
    for (int i = 3; i < 23; i++) {
        for (int j = 3; j < 77; j++) {
            gotoxy(j, i);
            printf(" ");
        }
    }
}

void guardar_posicion() {
    cuerpo[n][0] = x;
    cuerpo[n][1] = y;
    n++;
    if (n == tam) n = 1;
}

void pintar_cuerpo() {
    setColor(11); // Azul para el cuerpo de la serpiente
    for (int i = 1; i < tam; i++) {
        gotoxy(cuerpo[i][0], cuerpo[i][1]); printf("%c", 169); // Cambia el cuerpo por *
    }
    setColor(13); // Rojo para la cabeza de la serpiente
    gotoxy(x, y); printf("%c", 228); // Cambia la cabeza por O
    setColor(7); // Regresar a color blanco
}

void borrar_cuerpo() {
    gotoxy(cuerpo[n][0], cuerpo[n][1]); printf(" ");
}

void teclear() {
    if (kbhit()) {
        tecla = getch();
        switch (tecla) {
            case ARRIBA:
                if (dir != 2) dir = 1;
                break;
            case ABAJO:
                if (dir != 1) dir = 2;
                break;
            case DERECHA:
                if (dir != 4) dir = 3;
                break;
            case IZQUIERDA:
                if (dir != 3) dir = 4;
                break;
        }
    }
}

void comida() {
    for (int i = 0; i < NUM_COMIDA; i++) {
        // Si la serpiente está en la posición de una comida
        if (x == xc[i] && y == yc[i]) {
            tam++;
            puntuacion += 10;
            // Generar nueva posición aleatoria para este cuadro de comida
            xc[i] = (rand() % 73) + 4;
            yc[i] = (rand() % 19) + 4;
            setColor(4); // Amarillo para la comida
            gotoxy(xc[i], yc[i]); printf("%c", 254);
            setColor(7); // Regresar a color blanco
        }
    }
}

bool verificar_colision() {
    // Verifica si la serpiente ha chocado con las paredes
    if (y == 3 || y == 23 || x == 2 || x == 77) {
        vidas--;
        return true;
    }
    // Verifica si la serpiente choca consigo misma
    for (int j = tam - 1; j > 0; j--) {
        if (cuerpo[j][0] == x && cuerpo[j][1] == y) {
            vidas--;
            return true;
        }
    }
    return false;
}

void puntos() {
    gotoxy(3, 1); printf("Puntuacion: %d", puntuacion);
    gotoxy(20, 1); printf("Vidas: %d", vidas);
}

void cambiar_velocidad() {
    if (puntuacion == h * 20) {
        velocidad -= 10;
        h++;
    }
}

void reiniciar_posicion() {
    // Reinicia la posición de la cabeza de la serpiente
    x = 10;
    y = 12;
    dir = 3;
    tam = 10;
    n = 1;

    // Limpia el array que almacena las posiciones del cuerpo de la serpiente
    for (int i = 0; i < 200; i++) {
        cuerpo[i][0] = 0;
        cuerpo[i][1] = 0;
    }

    // Limpia la pantalla para borrar cualquier rastro de la serpiente
    limpiar_area_juego();
}

int main() {
    pintar();
    inicializar_comida();

    while (tecla != ESC && vidas > 0) {
        borrar_cuerpo();
        guardar_posicion();
        pintar_cuerpo();

        comida();
        puntos();
        cambiar_velocidad();
        teclear();
        teclear();

        // Movimiento de la serpiente
        if (dir == 1) y--;
        if (dir == 2) y++;
        if (dir == 3) x++;
        if (dir == 4) x--;

        // Verificar colisión
        if (verificar_colision()) {
            reiniciar_posicion();  // Reinicia la posición de la serpiente y limpia su rastro
            pintar();              // Redibuja el marco del área de juego
            inicializar_comida();  // Redibuja la comida
        }

        Sleep(velocidad);
    }

    gotoxy(30, 12); printf("Game Over"); // Mensaje de fin del juego
    getch(); // Espera a que el usuario presione una tecla sin mostrar texto adicional
    return 0;
}
