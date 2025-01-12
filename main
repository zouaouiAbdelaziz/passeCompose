import flet as ft  
import pygame  
import time
import sys
from pygame.base import *
# Initialiser pygame pour le son  
pygame.mixer.init()  

# Charger les sons  
correct_sound = pygame.mixer.Sound("correct.mp3")  # Assurez-vous d'avoir ce fichier  
incorrect_sound = pygame.mixer.Sound("incorrect.mp3")  # Assurez-vous d'avoir ce fichier  

# Questions par niveau  
questions_by_level = {  
    "Facile": [  
        {"question": "Quel est le participe passé du verbe 'manger'?", "options": ["mangé", "mange", "manges"], "correct": "mangé"},  
        {"question": "Quel auxiliaire est utilisé avec 'aller' au passé composé?", "options": ["avoir", "être"], "correct": "être"},  
        {"question": "Quel est le participe passé du verbe 'voir'?", "options": ["vu", "voir", "vues"], "correct": "vu"},  
        {"question": "Quel est le participe passé du verbe 'prendre'?", "options": ["pris", "prendre", "prenant"], "correct": "pris"},  
        {"question": "Quel est le participe passé du verbe 'faire'?", "options": ["fait", "faire", "faisant"], "correct": "fait"},  
        {"question": "Quel auxiliaire est utilisé avec 'venir' au passé composé?", "options": ["avoir", "être"], "correct": "être"},  
        {"question": "Quel est le participe passé du verbe 'écrire'?", "options": ["écrit", "écrire", "écrivant"], "correct": "écrit"},  
        {"question": "Quel est le participe passé du verbe 'lire'?", "options": ["lu", "lire", "lisant"], "correct": "lu"},  
        {"question": "Quel auxiliaire est utilisé avec 'mourir' au passé composé?", "options": ["avoir", "être"], "correct": "être"},  
        {"question": "Quel est le participe passé du verbe 'dire'?", "options": ["dit", "dire", "disant"], "correct": "dit"},  
    ],  
    "Moyen": [  
        {"question": "Quel est le participe passé du verbe 'finir'?", "options": ["fini", "finis", "finissant"], "correct": "fini"},  
        {"question": "Quel auxiliaire est utilisé avec 'naître' au passé composé?", "options": ["avoir", "être"], "correct": "être"},  
        {"question": "Quel est le participe passé du verbe 'choisir'?", "options": ["choisi", "choisissant", "choisir"], "correct": "choisi"},  
        {"question": "Quel est le participe passé du verbe 'grandir'?", "options": ["grandi", "grandissant", "grandir"], "correct": "grandi"},  
        {"question": "Quel auxiliaire est utilisé avec 'descendre' au passé composé?", "options": ["avoir", "être"], "correct": "être"},  
        {"question": "Quel est le participe passé du verbe 'réussir'?", "options": ["réussi", "réussissant", "réussir"], "correct": "réussi"},  
        {"question": "Quel auxiliaire est utilisé avec 'sortir' au passé composé?", "options": ["avoir", "être"], "correct": "être"},  
        {"question": "Quel est le participe passé du verbe 'attendre'?", "options": ["attendu", "attendre", "attendant"], "correct": "attendu"},  
        {"question": "Quel est le participe passé du verbe 'répondre'?", "options": ["répondu", "répondre", "répondant"], "correct": "répondu"},  
        {"question": "Quel auxiliaire est utilisé avec 'monter' au passé composé?", "options": ["avoir", "être"], "correct": "être"},  
    ],  
    "Difficile": [  
        {"question": "Quel est le participe passé du verbe 'apprendre'?", "options": ["appris", "apprenant", "apprendre"], "correct": "appris"},  
        {"question": "Quel auxiliaire est utilisé avec 'venir' au passé composé?", "options": ["avoir", "être"], "correct": "être"},  
        {"question": "Quel est le participe passé du verbe 'comprendre'?", "options": ["compris", "comprenant", "comprendre"], "correct": "compris"},  
        {"question": "Quel est le participe passé du verbe 'mettre'?", "options": ["mis", "mettant", "mettre"], "correct": "mis"},  
        {"question": "Quel auxiliaire est utilisé avec 'retourner' au passé composé?", "options": ["avoir", "être"], "correct": "être"},  
        {"question": "Quel est le participe passé du verbe 'voir'?", "options": ["vu", "voir", "vues"], "correct": "vu"},  
        {"question": "Quel est le participe passé du verbe 'ouvrir'?", "options": ["ouvert", "ouvrant", "ouvrir"], "correct": "ouvert"},  
        {"question": "Quel auxiliaire est utilisé avec 'tomber' au passé composé?", "options": ["avoir", "être"], "correct": "être"},  
        {"question": "Quel est le participe passé du verbe 'dire'?", "options": ["dit", "dire", "disant"], "correct": "dit"},  
        {"question": "Quel est le participe passé du verbe 'boire'?", "options": ["bu", "boire", "buvant"], "correct": "bu"},  
    ]  
}  

def main(page: ft.Page):
    page.title = "Quiz sur le Passé Composé"
    page.window.width = 390
    page.window.height = 740

    # Variable pour stocker le niveau actuel et les questions
    current_level = "Facile"
    current_question = 0
    score = 0
    

    def show_splash_screen():
        # Jouer le son de démarrage
        pygame.mixer.music.load("music_level2.mp3")  # Assurez-vous que ce fichier existe
         # Diminuer le volume du son de fond
       
        pygame.mixer.music.play() 
        pygame.mixer.music.set_volume(0.5)  # Diminuer le volume à 20%
        splash_image = ft.Image(src="splash_image.png", width=200, opacity=0.0,border_radius=10,height=300)
        splash_text = ft.Text(
            "Bienvenue au Quiz!", size=30, weight=ft.FontWeight.BOLD, opacity=0.0, 
        )
        
        splash_loading = ft.Text("Chargement...", size=20, opacity=0.0)

        splash = ft.Column(
            [splash_text, splash_image, splash_loading],
            alignment=ft.MainAxisAlignment.CENTER,
            horizontal_alignment=ft.CrossAxisAlignment.CENTER,
        )

        page.add(splash)
        page.update()

        # Animer l'image et le texte
        splash_image.opacity = 1.0
        splash_text.opacity = 1.0
        splash_loading.opacity = 1.0
        page.update()

        time.sleep(2)  # Temps d'affichage du splash screen
        page.controls.clear()
        build_level_selection()

    def build_level_selection():
        page.add(
        ft.Column(
            [
                ft.Text("Choisissez un niveau:", size=30, weight=ft.FontWeight.BOLD),
                ft.FilledButton(
                    "Facile",
                    on_click=lambda e: start_quiz("Facile"),
                    style=ft.ButtonStyle(
                        bgcolor="#00FF00",
                        shape=ft.RoundedRectangleBorder(radius=10),
                        padding=ft.Padding(20, 10, 20, 10),
                        text_style=ft.TextStyle(size=25)
                    ),
                    width=200,
                    height=50
                ),
                ft.FilledButton(
                    "Moyen",
                    on_click=lambda e: start_quiz("Moyen"),
                    style=ft.ButtonStyle(
                        bgcolor="#FFA500",
                        shape=ft.RoundedRectangleBorder(radius=10),
                        padding=ft.Padding(20, 10, 20, 10),
                        text_style=ft.TextStyle(size=25)
                    ),
                    width=200,
                    height=50
                ),
                ft.FilledButton(
                    "Difficile",
                    on_click=lambda e: start_quiz("Difficile"),
                    style=ft.ButtonStyle(
                        bgcolor="#FF0000",
                        shape=ft.RoundedRectangleBorder(radius=10),
                        padding=ft.Padding(20, 10, 20, 10),
                        text_style=ft.TextStyle(size=25)
                    ),
                    width=200,
                    height=50
                ),
                ft.FilledButton(
                    "Fermer",
                    on_click=lambda e: page.window_close(),
                    style=ft.ButtonStyle(
                        bgcolor="#0000FF",
                        shape=ft.RoundedRectangleBorder(radius=10),
                        padding=ft.Padding(20, 10, 20, 10),
                        text_style=ft.TextStyle(size=25)
                    ),
                    width=200,
                    height=50
                ),
            ],
            expand=True,
            alignment=ft.MainAxisAlignment.CENTER,          
            horizontal_alignment=ft.CrossAxisAlignment.CENTER,
            spacing=10
        )
    )

    # Animation d'entrée pour les boutons
    for control in page.controls:
        control.opacity = 0.0
        page.update()
        control.animate(opacity=1.0, duration=2000)

    page.update()

    def start_quiz(level):
        nonlocal current_question, score
        current_level = level
        current_question = 0
        score = 0
        build_ui()

    def build_ui():
        page.controls.clear()
        question = questions_by_level[current_level][current_question]
        
        # Créer un conteneur avec une image d'arrière-plan
        background_image = ft.Container(
            content=ft.Column(
                [
                    ft.Text(f"Niveau: {current_level}", size=25, text_align=ft.TextAlign.CENTER, color="black"),  # Affichage du niveau
                    ft.Text(question["question"], size=25, text_align=ft.TextAlign.CENTER, color="black"),
                    *[ft.FilledButton(option, on_click=check_answer, width=200, height=50, style=ft.ButtonStyle(
                        bgcolor="#2196F3",
                        shape=ft.RoundedRectangleBorder(radius=10),
                        padding=ft.Padding(20, 10, 20, 10),
                        text_style=ft.TextStyle(size=25)
                    )) for option in question["options"]],
                    ft.Text(f"Score: {score}", size=25, text_align=ft.TextAlign.CENTER, color="yellow"),
                ],
                alignment=ft.MainAxisAlignment.CENTER,
                horizontal_alignment=ft.CrossAxisAlignment.CENTER,
                spacing=20,
            ),
            alignment=ft.alignment.center,
            padding=10,
            width=page.window.width,
            height=page.window.height,
            bgcolor="00000000" #vous que le fond est transparent
        )

        # Ajouter l'image d'arrière-plan
        background = ft.Image(src="background_image2.jpg", width=page.window.width, height=page.window.height)

        # Créer un conteneur principal qui contient l'image et le contenu
        main_container = ft.Container(
            content=ft.Stack(
                [
                    background,  # Image d'arrière-plan
                    background_image  # Contenu principal
                ]
            ),
            width=page.window.width,
            height=page.window.height
        )

        page.add(main_container)
        page.update()
        
    def check_answer(e):
        nonlocal score, current_question
        selected_option = e.control.text
        correct_answer = questions_by_level[current_level][current_question]["correct"]

         # Diminuer le volume du son de fond
        pygame.mixer.music.set_volume(0.2)  # Diminuer le volume à 20%

        if selected_option == correct_answer:
           score += 1
           correct_sound.play()  # Joue le son correct
        else:
           incorrect_sound.play()  # Joue le son incorrect

        current_question += 1

        if current_question < len(questions_by_level[current_level]):
           build_ui()
        else:
           show_results()

    # Rétablir le volume du son de fond après un court délai
        time.sleep(1.5)  # Attendre un moment pour que le son soit joué
        pygame.mixer.music.set_volume(0.4)  # Rétablir le volume à 100%


    def show_results():
        page.controls.clear()
        page.add(
           ft.Container(
            content=ft.Column(
                [
                    ft.Text("Quiz terminé!", size=24, text_align=ft.TextAlign.CENTER, style="red"),
                    ft.Text(f"Votre score: {score}/{len(questions_by_level[current_level])}", size=20, text_align=ft.TextAlign.CENTER),
                    ft.FilledButton("Recommencer", on_click=reset_quiz, width=250, height=50),  # Bouton pour recommencer
                    ft.FilledButton("Niveau Suivant", on_click=next_level, width=250, height=50)  # Nouveau bouton pour le niveau suivant
                ],
                alignment=ft.MainAxisAlignment.CENTER,
            ),
            alignment=ft.alignment.center,
            padding=50,
            width=page.window.width,
            height=page.window.height,
        )
    )
    page.update()


    def reset_quiz(e):
        nonlocal current_question, score
        current_question = 0
        score = 0
        build_level_selection()  # Retour à la sélection de niveau

    # Afficher l'écran de démarrage
    show_splash_screen()
    
    def next_level(e):
        nonlocal current_level, current_question
        levels = list(questions_by_level.keys())
        current_index = levels.index(current_level)
    
        if current_index + 1 < len(levels):
            current_level = levels[current_index + 1]
            print("level" + current_level)
            current_question = 0  # Réinitialiser la question pour le nouveau niveau
            score = 0  # Réinitialiser le score
            build_ui()  # Construire l'interface pour le nouveau niveau
        else:
            ft.alert("Vous avez terminé tous les niveaux!")  # Alerte si c'est le dernier niveau
            reset_quiz(e)  # Retour à la sélection de niveau


ft.app(target=main)
