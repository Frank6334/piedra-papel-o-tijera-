import pygame
import random

# Inicializar Pygame
pygame.init()

# Dimensiones de la ventana
ANCHO = 600
ALTO = 400
pantalla = pygame.display.set_mode((ANCHO, ALTO))
pygame.display.set_caption("Piedra, Papel o Tijera - Juego Pixelado")

# Colores
NEGRO = (0, 0, 0)
ROJO = (255, 0, 0)
AZUL = (0, 0, 255)
VERDE = (0, 255, 0)
BLANCO = (255, 255, 255)

# Fuente para el texto
fuente = pygame.font.SysFont("Arial", 30)

# Opciones
opciones = ["Piedra", "Papel", "Tijera"]

# Función para dibujar los píxeles de Piedra, Papel o Tijera
def dibujar_piedra(x, y):
    pygame.draw.rect(pantalla, ROJO, (x, y, 50, 50))  # Piedra representada por un cuadrado rojo

def dibujar_papel(x, y):
    pygame.draw.rect(pantalla, AZUL, (x, y, 50, 100))  # Papel representado por un rectángulo azul

def dibujar_tijera(x, y):
    pygame.draw.line(pantalla, VERDE, (x, y), (x + 50, y + 50), 5)  # Tijera representada por una línea verde

# Función para mostrar el texto en pantalla
def mostrar_texto(texto, color, x, y):
    texto_renderizado = fuente.render(texto, True, color)
    pantalla.blit(texto_renderizado, (x, y))

# Función para mostrar una ventana de reintentar
def mostrar_ventana_reintentar():
    pantalla.fill(NEGRO)
    mostrar_texto("¡Fallaste! ¿Quieres intentarlo de nuevo?", BLANCO, 100, 150)
    mostrar_texto("Haz clic en cualquier lugar para continuar.", BLANCO, 100, 200)
    pygame.display.flip()

# Función principal del juego
def juego():
    ejecutar = True
    eleccion_usuario = None
    eleccion_computadora = None
    ganador = None

    while ejecutar:
        pantalla.fill(NEGRO)
        
        # Mostrar instrucciones
        mostrar_texto("Elige Piedra, Papel o Tijera:", BLANCO, 150, 20)
        
        # Crear botones (piedra, papel, tijera)
        # Piedra
        pygame.draw.rect(pantalla, ROJO, (50, 100, 150, 50))
        mostrar_texto("Piedra", BLANCO, 90, 110)
        
        # Papel
        pygame.draw.rect(pantalla, AZUL, (50, 180, 150, 50))
        mostrar_texto("Papel", BLANCO, 90, 190)
        
        # Tijera
        pygame.draw.rect(pantalla, VERDE, (50, 260, 150, 50))
        mostrar_texto("Tijera", BLANCO, 90, 270)

        # Mostrar elección de la computadora si el jugador ha elegido
        if eleccion_usuario:
            if eleccion_computadora == "Piedra":
                dibujar_piedra(400, 100)
            elif eleccion_computadora == "Papel":
                dibujar_papel(400, 100)
            elif eleccion_computadora == "Tijera":
                dibujar_tijera(400, 100)

        # Verificar eventos
        for evento in pygame.event.get():
            if evento.type == pygame.QUIT:
                ejecutar = False
            elif evento.type == pygame.MOUSEBUTTONDOWN:
                x, y = pygame.mouse.get_pos()
                
                # Seleccionar Piedra
                if 50 <= x <= 200 and 100 <= y <= 150:
                    eleccion_usuario = "Piedra"
                # Seleccionar Papel
                elif 50 <= x <= 200 and 180 <= y <= 230:
                    eleccion_usuario = "Papel"
                # Seleccionar Tijera
                elif 50 <= x <= 200 and 260 <= y <= 310:
                    eleccion_usuario = "Tijera"
                
                # Verificar si el jugador ha hecho una elección
                if eleccion_usuario:
                    # Elección aleatoria de la computadora
                    eleccion_computadora = random.choice(opciones)

                    # Verificar quién ganó
                    if eleccion_usuario == eleccion_computadora:
                        ganador = "Empate"
                    elif (eleccion_usuario == "Piedra" and eleccion_computadora == "Tijera") or \
                         (eleccion_usuario == "Papel" and eleccion_computadora == "Piedra") or \
                         (eleccion_usuario == "Tijera" and eleccion_computadora == "Papel"):
                        ganador = "¡Felicidades, ganaste!"
                    else:
                        ganador = "Fallaste, ¿quieres intentarlo de nuevo?"

                    # Mostrar el resultado
                    mostrar_texto(f"Tú elegiste: {eleccion_usuario}", BLANCO, 50, 340)
                    mostrar_texto(f"La computadora eligió: {eleccion_computadora}", BLANCO, 50, 370)
                    mostrar_texto(ganador, BLANCO, 50, 400)

                    # Si perdiste, mostrar la ventana de reintentar
                    if ganador == "Fallaste, ¿quieres intentarlo de nuevo?":
                        mostrar_ventana_reintentar()

                        # Esperar hasta que el jugador haga clic en cualquier lugar para reiniciar
                        esperar_click = True
                        while esperar_click:
                            for evento in pygame.event.get():
                                if evento.type == pygame.QUIT:
                                    esperar_click = False
                                    ejecutar = False
                                elif evento.type == pygame.MOUSEBUTTONDOWN:
                                    # Reiniciar el juego después de hacer clic
                                    eleccion_usuario = None
                                    eleccion_computadora = None
                                    ganador = None
                                    esperar_click = False

        pygame.display.flip()
        pygame.time.Clock().tick(30)

# Ejecutar el juego
juego()
pygame.quit()
