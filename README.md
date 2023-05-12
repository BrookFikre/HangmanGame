# HangmanGame
import numpy as np
import skimage, skimage.io
import matplotlib.pyplot as plt
"""
Name: Brook Fikre
Section: 0105
UID: bfikre
"""
class Hangman(): #the class that hangman is in

    #initializing the start of the game
    def __init__(self):
        print("Welcome to Hangman! Are you ready to die!!!")
        print(''' 
                     _                                             
                    | |                                            
                    | |__   __ _ _ __   __ _ _ __ ___   __ _ _ __  
                    | '_ \ / _` | '_ \ / _` | '_ ` _ \ / _` | '_ \ 
                    | | | | (_| | | | | (_| | | | | | | (_| | | | |
                    |_| |_|\__,_|_| |_|\__, |_| |_| |_|\__,_|_| |_|
                                        __/ |                      
                                       |___/    ''')
        playing = input(str("Would you like to start playing? (y/n): "))

        #start conditions:
        if playing == 'y':
            print("Would you like to play Single player or Multiplayer?")
            setting = input(str("Type SINGLE or MULTIPLAYER: "))
            if setting == 'SINGLE':
                print("Loading nooses, thiefs, lunatics... preparing your death bed!")
                self.core_game()
            elif setting == 'MULTIPLAYER':
                print("Loading nooses, thiefs, lunatics... preparing your death bed!")
                self.multiplayer()


        elif playing == 'n':
            print("Bye bye now...")
            exit()
        else:
            print("Choose one of the available options! Or else.....")
            self.__init__()

    #Holds the core of the game
    def core_game(self):
        print("Now that you have sealed your fate. Choose a difficulty")
        #difficulty = input(str("Your options are (Easy, Hard, or insane): "))
        count = 0  # allows the loop to exit
        diff = 0  # represents a value corresponding to the difficulty

        #make user choose difficulty and stores it
        while count == 0:
            difficulty = input(str("Your options are (EASY, HARD, or INSANE): "))
            if difficulty == 'EASY':
                count = 1
                diff = 1
            elif difficulty == 'HARD':
                count = 1
                diff = 2
            elif difficulty == 'INSANE':
                count = 1
                diff = 3
            else:
                print("Choose a valid difficulty")


        #generaring a random word for each difficulty
        easy_index = np.random.randint(0, 5)
        hard_index = np.random.randint(0, 5)
        insane_index = np.random.randint(0, 5)
        the_word = ""
        multiplier = 1

        #holds the list of words in each difficulty
        list_of_easy = ["able", "camera", "pizza", "circle", "find", "stand"]
        list_of_hard = ["advertisement", "knowledge", "representative", "tenacity", "android", "obscure"]
        list_of_insane = ["insatiable", "facetious", "arcane", "eloquent", "extol", "gratuitous"]

        #assigns difficulty a word bank
        if diff == 1:
            the_word = the_word + str(list_of_easy[easy_index])
            multiplier = 1
        elif diff == 2:
            the_word = the_word + str(list_of_hard[hard_index])
            multiplier = 2
        elif diff == 3:
            the_word = the_word + str(list_of_insane[insane_index])
            multiplier = 3

        #actual mechancis of the game
        guesses = 0
        used_letters = ""
        progress = np.empty(len(the_word), dtype=str) #makes an array of the size of the random word
        print("progress is " , progress)
        score = 0

        while guesses < 6:
            guess = input(str("Guess a letter: "))
            if guess in the_word:
                print("Your guess was right!")
                self.graphics(guesses)
                used_letters+= "," + guess
                result = self.progress_updater(guess, the_word, progress)
                print("Progress: " + result)
                print("Letter used: " + used_letters)
                if result == the_word:
                    score = len(the_word) * multiplier - guesses*multiplier
                    if score < 0:
                        score = 0

                    print("You win!")
                    print("Your final score was ", score)
                    # create a new figure
                    fig = plt.figure()

                    # add a text label to the figure
                    plt.text(0.5, 0.5, "You LIVE. Congrats!", fontsize=20, ha="center")

                    # show the figure
                    plt.show()
                    return
            elif guess not in the_word:
                print("incorrect")
                guesses+=1
                used_letters += "," + guess
                self.graphics(guesses)
                print(
                    "Progress: " + "".join(progress))
                print(
                    "Letter used: " + used_letters)



        #making the graphics for the game
    def graphics(self,guesses):
        #With each wrong guess another body part is drawn
        if guesses == 0:
            print(
                "________      ")
            print(
                "|      |      ")
            print(
                "|             ")
            print(
                "|             ")
            print(
                "|             ")
            print(
                "|             ")
        elif guesses == 1:
            print(
                "________      ")
            print(
                "|      |      ")
            print(
                "|      0      ")
            print(
                "|             ")
            print(
                "|             ")
            print(
                "|             ")
        elif guesses == 2:
            print(
                "________      ")
            print(
                "|      |      ")
            print(
                "|      0      ")
            print(
                "|     /       ")
            print(
                "|             ")
            print(
                "|             ")
        elif guesses == 3:
            print(
                "________      ")
            print(
                "|      |      ")
            print(
                "|      0      ")
            print(
                "|     /|      ")
            print(
                "|             ")
            print(
                "|             ")
        elif guesses == 4:
            print(
                "________      ")
            print(
                "|      |      ")
            print(
                "|      0      ")
            print(
                "|     /|\     ")
            print(
                "|             ")
            print(
                "|             ")
        elif guesses == 5:
            print(
                "________      ")
            print(
                "|      |      ")
            print(
                "|      0      ")
            print(
                "|     /|\     ")
            print(
                "|     /       ")
            print(
                "|             ")
        else:
            print(
                "________      ")
            print(
                "|      |      ")
            print(
                "|      0      ")
            print(
                "|     /|\     ")
            print(
                "|     / \     ")
            print(
                "|             ")
            print(
                "The noose tightens around your neck, and you feel the")
            print(
                "sudden urge to urinate.")
            print(
                "GAME OVER!")
            # create a new figure
            fig = plt.figure()

            # add a text label to the figure
            plt.text(0.5, 0.5, "You Are DEAD!!", fontsize=20, ha="center")

            # show the figure
            plt.show()
            self.__init__()

            #updates the progress bar
    def progress_updater(self, guess, the_word, progress):
        i = 0
        while i < len(the_word):
            if guess == the_word[i]:
                progress[i] = guess
                i += 1
            else:
                i += 1

        return "".join(progress)

    #Holds the multiplayer version of the game
    def multiplayer(self):
        # prompt the first player to choose a word
        word = input("Player 1, choose a word: ")

        # create a list of underscores to represent the letters in the chosen word
        # (for example, if the word is "python", the list would be ["_", "_", "_", "_", "_", "_"])
        underscores = ["_"] * len(word)

        # keep track of the number of incorrect guesses
        incorrect_guesses = 0

        # keep track of the letters that have been guessed
        letters_guessed = []

        # keep playing the game until the player wins or loses
        while True:
            # print the current state of the game
            print("Word: ", " ".join(underscores))
            print("Incorrect guesses: ", incorrect_guesses)
            print("Letters guessed: ", " ".join(letters_guessed))

            # prompt the second player to guess a letter
            guess = input("Player 2, guess a letter: ")

            # check if the guess is a letter that has not been guessed before
            if guess in letters_guessed:
                print("You already guessed that letter. Try again.")
                continue
            else:
                letters_guessed.append(guess)

            # check if the guess is in the chosen word
            if guess in word:
                # if the guess is correct, update the list of underscores to show the correct letter
                for i in range(len(word)):
                    if word[i] == guess:
                        underscores[i] = guess

                # check if the player has won by guessing all the letters in the word
                if "_" not in underscores:
                    print("Player 2 wins! The word was ", word)
                    # create a new figure
                    fig = plt.figure()

                    # add a text label to the figure
                    plt.text(0.5, 0.5, "You LIVE. Congrats!", fontsize=20, ha="center")

                    # show the figure
                    plt.show()
                    break
            else:
                # if the guess is incorrect, increment the number of incorrect guesses
                incorrect_guesses += 1
                self.graphics(incorrect_guesses)

                # check if the player has lost by guessing too many incorrect letters
                if incorrect_guesses >= 6:
                    print("Player 2 loses! The word was ", word)
                    # create a new figure
                    fig = plt.figure()

                    # add a text label to the figure
                    plt.text(0.5, 0.5, "You Are DEAD!!", fontsize=20, ha="center")

                    # show the figure
                    plt.show()
                    break


#Makes the game run until n is input

while True:
    Hangman()

    # ask the players if they want to play again
    play_again = input("Do you want to play again? (y/n) ")
    if play_again == "n":
        break