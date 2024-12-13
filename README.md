# android-app-tick-tac-toe-with-python
pip install kivy

pip install buildozer
Create the Tic-Tac-Toe Game in Python using Kivy: Create a new Python file, say tictactoe.py, and add the following code:

python
Copy code
from kivy.app import App
from kivy.uix.gridlayout import GridLayout
from kivy.uix.button import Button
from kivy.uix.label import Label
from kivy.uix.popup import Popup
from kivy.uix.boxlayout import BoxLayout

class TicTacToeApp(App):
    def build(self):
        self.board = [None] * 9  # 3x3 grid
        self.turn = 'X'  # 'X' starts
        self.buttons = []
        
        layout = GridLayout(cols=3, padding=10, spacing=10)
        
        for i in range(9):
            btn = Button(font_size=40, on_press=self.on_button_press)
            self.buttons.append(btn)
            layout.add_widget(btn)

        self.label = Label(text="Player X's turn", font_size=24, size_hint_y=None, height=50)
        
        main_layout = BoxLayout(orientation='vertical')
        main_layout.add_widget(self.label)
        main_layout.add_widget(layout)
        
        return main_layout

    def on_button_press(self, instance):
        if instance.text == '':  # Button is empty, can be pressed
            instance.text = self.turn
            self.check_winner()
            self.turn = 'O' if self.turn == 'X' else 'X'  # Switch turn
            self.label.text = f"Player {self.turn}'s turn"
    
    def check_winner(self):
        # Define all possible winning combinations
        winning_combinations = [
            [0, 1, 2], [3, 4, 5], [6, 7, 8],  # horizontal
            [0, 3, 6], [1, 4, 7], [2, 5, 8],  # vertical
            [0, 4, 8], [2, 4, 6]  # diagonal
        ]
        
        for combo in winning_combinations:
            if self.buttons[combo[0]].text == self.buttons[combo[1]].text == self.buttons[combo[2]].text != '':
                self.show_winner(self.buttons[combo[0]].text)
                return

        if all(btn.text != '' for btn in self.buttons):  # Check if it's a draw
            self.show_winner('Draw')
    
    def show_winner(self, winner):
        if winner == 'Draw':
            winner_text = "It's a draw!"
        else:
            winner_text = f"Player {winner} wins!"
        
        popup_layout = BoxLayout(orientation='vertical', padding=10)
        popup_label = Label(text=winner_text, font_size=24)
        popup_layout.add_widget(popup_label)

        popup_btn = Button(text="Play Again", size_hint_y=None, height=50)
        popup_btn.bind(on_press=self.reset_game)
        popup_layout.add_widget(popup_btn)
        
        self.popup = Popup(title='Game Over', content=popup_layout, size_hint=(None, None), size=(400, 300))
        self.popup.open()

    def reset_game(self, instance):
        # Reset the game
        for btn in self.buttons:
            btn.text = ''
        self.turn = 'X'
        self.label.text = "Player X's turn"
        self.popup.dismiss()

if __name__ == '__main__':
    TicTacToeApp().run()
#Code Explanation:
Grid Layout: The game board is represented as a 3x3 grid with Button widgets. Each button corresponds to a cell in the Tic-Tac-Toe board.
Game Logic: The game alternates between players 'X' and 'O'. When a player clicks a button, their symbol (either 'X' or 'O') is placed there.
Winning Check: After each move, the app checks for a winning combination. If a player wins, a popup shows up, and the game can be restarted.
Popup: A Popup widget is used to display the winner (or if it's a draw) and allow the player to reset the game.
Test the Game Locally: You can run the game on your local machine by using the following command:

bash

python tictactoe.py
This will open a window with the Tic-Tac-Toe game, where you can play between two players.

Build the APK for Android: To package your app for Android, you need Buildozer.

First, initialize Buildozer in your project directory:

bash

buildozer init
This will create a buildozer.spec file.

Edit the buildozer.spec file: Open the buildozer.spec file and adjust the following parameters:

Set package.name and package.domain for your app.
Ensure source.include_exts = py,png,jpg,kv,atlas is set to include all necessary files.
Set android.permissions = INTERNET if your app requires internet access.
Build the APK: Now you can build the APK for Android by running:

bash

buildozer -v android debug
This will take some time, depending on your system, as Buildozer will set up an Android environment and compile the app.

Install the APK on your Android device: Once the APK is built, you can install it on your Android device by either manually transferring the APK or using the following command:

bash

buildozer android deploy run
Notes:
If you're using Windows, you may need to set up a Linux environment (like WSL) for Buildozer to work properly, or you can use a Linux-based system for development.
You can expand this app by adding features like AI for single-player mode or customizing the design further with Kivy’s theming features.
Optional: Use KivyMD for Material Design
For a more polished UI, you can use KivyMD (Kivy Material Design) to give the app a more modern appearance. This library provides material design widgets and layouts.

bash

pip install kivymd
With KivyMD, you can make the app look more like a native Android application.run code in console
