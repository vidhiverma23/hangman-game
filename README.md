# hangman-game
import random
import string

def get_available_letters(letter_list):
    available_letters = string.ascii_lowercase
    for letter in letter_list:
        if letter in available_letters:
            available_letters = available_letters.replace(letter, '_')
    return available_letters

def choose_word(word_list, level):
    if level == 1:
        word_list_level1 = ['apple', 'banana', 'cherry', 'date', 'fig', 'grape']
    elif level == 2:
        word_list_level2 = ['kiwi', 'lemon', 'mango', 'orange', 'peach', 'pear', 'pineapple', 'plum', 'watermelon']
    else:
        word_list_level3 = ['blueberry', 'boysenberry', 'cranberry', 'elderberry', 'gooseberry', 'raspberry', 'strawberry']

    word = random.choice(word_list_level3 if level == 3 else (word_list_level2 if level == 2 else word_list_level1))
    return word

def is_word_guessed(secret_word, letters_guessed):
    for letter in secret_word:
        if letter not in letters_guessed:
            return False
    return True

def main():
    word_list = ['apple', 'banana', 'cherry', 'date', 'elderberry', 'fig', 'grape', 'honeydew', 'kiwi', 'lemon', 'blueberry', 'boysenberry', 'cranberry', 'elderberry', 'gooseberry', 'raspberry', 'strawberry', 'mango', 'orange', 'peach', 'pear', 'pineapple', 'plum', 'watermelon']
    levels = ["Easy", "Medium", "Hard"]

    print("Welcome to the Advanced Hangman Game!")
    while True:
        try:
            level = int(input(f"Choose a level ({levels[0]} - Easy, {levels[1]} - Medium, {levels[2]} - Hard): "))
            if level not in [1, 2, 3]:
                raise ValueError
            break
        except ValueError:
            print("Invalid input. Please enter a number between 1 and 3.")

    secret_word = choose_word(word_list, level)
    guessed_letters = []
    attempts_left = 6
    print(f"The word has {len(secret_word)} letters.")

    while attempts_left > 0:
        print(f"You have {attempts_left} attempts left.\nAvailable letters: {get_available_letters(guessed_letters)}\n")
        guess = input("Enter your guess: ").lower()

        if len(guess) != 1:
            print("Invalid input. Please enter a single letter.")
            continue
        elif guess in guessed_letters:
            print("You have already guessed this letter.")
            continue

        guessed_letters.append(guess)
        if guess not in secret_word:
            attempts_left -= 1
        if is_word_guessed(secret_word, guessed_letters):
            print(f"Congratulations! You've won! The word was {secret_word}.")
            break

    if attempts_left == 0:
        print(f"You have run out of attempts. The word was {secret_word}.")

if __name__ == "__main__":
    main()
