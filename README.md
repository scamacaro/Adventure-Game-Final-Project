# Adventure-Game-Final-Project
"""
Sanyerlis Camacaro - CSC235 - Sancamac@uat.edu Assignment:
Final Project: OOP Application and Presentation Video.

"Adventure Game"

This code demonstrates how to:
Create a new console project.
Create a Simulation.
Add at least 5 functions to your application in addition to the main.
Add at least one function which takes an argument.
Add at least one function which returns a value.
Your main function should control all the other functions.
An opening description of the application and instructions for the user are displayed on the screen.
Your application must take User Input.
Your application must Output to the Display.
Your application must contain variables with at least two different data types.
At least 3 classes total.
Use at least one List.

The Game functions demonstrates:

- `start_game` is the main function that starts and controls the game.
- `hall`, `kitchen`, `library`, `garden` and `basement` are functions for different game scenarios.
They all take a game state as an argument and return an updated game state. This way, they can affect the game
and pass on the results of their actions.
- `if __name__ == "__main__":` is a special construct that checks if the script is being run directly. If it is,
it starts the game by calling the `start_game` function.

Each scenario function prints out a description of the current location and asks for input. Depending on the input
,the function may change the game state (for example, change the location or set `has_treasure` to `True`).
The game_state dictionary now includes an "inventory" key, which holds the player's inventory as a list.
In the kitchen() function, if the player opens the fridge and finds the treasure, the "treasure" is added to the
player's inventory using game_state["inventory"].append("treasure").
At the end of the game, the player's inventory is displayed using print("Inventory:", game_state["inventory"]).
"""


class Room:
    def __init__(self, name, description):
        self.name = name
        self.description = description


class Hall(Room):
    def __init__(self):
        super().__init__("hall", "You're in the hall. There are four doors: kitchen, library, garden, and basement.")


class Kitchen(Room):
    def __init__(self):
        super().__init__("kitchen", "You're in the kitchen. You can see a fridge and a door leading back to the hall.")


class Library(Room):
    def __init__(self):
        super().__init__("library",
                         "You're in the library. There are many books on the shelves and a door leading back to the hall.")


class Garden(Room):
    def __init__(self):
        super().__init__("garden",
                         "You're in the garden. There are many beautiful plants and a door leading back to the hall.")


class Basement(Room):
    def __init__(self):
        super().__init__("basement",
                         "You're in the basement. It's dark and there are many boxes around. There's also a staircase leading back to the hall.")


def start_game():
    """
    The main function that starts the game. It uses other functions to control the flow of the game.
    """
    print("****** Welcome to the Adventure Game! ******")
    print("\nIn this game, your goal is to find the hidden treasure.")
    print("\nYou will navigate through different rooms, each with its own options for actions.")
    print("\nThe game starts in a hall with doors leading to different rooms.")
    print("\nEach room is an opportunity to find where the treasure might be hidden,")
    print("or return to the hall.")
    print("\nTo make a choice, simply type your desired action when prompted.")
    print("\nAre you ready? Let's start the adventure!")

    # Get the player's name
    name = input("\nWhat is your name? ")

    # Setting initial game state
    game_state = {
        "location": Hall(),
        "has_treasure": False,
        "score": 0,
        "inventory": []  # Player's inventory as a list
    }

    # Game loop
    while not game_state["has_treasure"]:
        if game_state["location"].name == "hall":
            game_state = hall(game_state)
        elif game_state["location"].name == "kitchen":
            game_state = kitchen(game_state)
        elif game_state["location"].name == "library":
            game_state = library(game_state)
        elif game_state["location"].name == "garden":
            game_state = garden(game_state)
        elif game_state["location"].name == "basement":
            game_state = basement(game_state)

    # Display victory message with the player's name and total score
    print("\nCongratulations, " + name + "! You've found the treasure and won the game!")
    print("\nTotal Winning Score: " + str(game_state["score"]))
    print("Inventory:", game_state["inventory"])


def hall(game_state):
    """
    Function for the hall scenario. The player can enter different doors.
    """
    print("\n" + game_state["location"].description)
    choice = input("Where do you want to go? (kitchen/library/garden/basement) ")

    if choice == "kitchen":
        game_state["location"] = Kitchen()
    elif choice == "library":
        game_state["location"] = Library()
    elif choice == "garden":
        game_state["location"] = Garden()
    elif choice == "basement":
        game_state["location"] = Basement()

    return game_state


def kitchen(game_state):
    """
    Function for the kitchen scenario. The player can return to the hall or open the fridge.
    The treasure is in the fridge.
    """
    print("\n" + game_state["location"].description)
    choice = input("What do you want to do? (open fridge/go back) ")

    if choice == "open fridge":
        game_state["has_treasure"] = True  # Player has found the treasure
        game_state["score"] += 100  # Increase the player's score by 100
        game_state["inventory"].append("treasure")  # Add the treasure to the player's inventory
    elif choice == "go back":
        game_state["location"] = Hall()

    return game_state


def library(game_state):
    """
    Function for the library scenario. The player can return to the hall or read a book. Reading a book does nothing.
    """
    print("\n" + game_state["location"].description)
    choice = input("What do you want to do? (read book/go back) ")

    if choice == "read book":
        print("You pick up a book and start reading. It's interesting but doesn't help you find the treasure.")
    elif choice == "go back":
        game_state["location"] = Hall()

    return game_state


def garden(game_state):
    """
    Function for the garden scenario. The player can return to the hall or water the plants.
    Watering the plants does nothing.
    """
    print("\n" + game_state["location"].description)
    choice = input("What do you want to do? (water plants/go back) ")

    if choice == "water plants":
        print("You water the plants. They seem to appreciate it, but it doesn't help you find the treasure.")
    elif choice == "go back":
        game_state["location"] = Hall()

    return game_state


def basement(game_state):
    """
    Function for the basement scenario. The player can return to the hall or search the boxes.
    Searching the boxes might reveal the treasure.
    """
    print("\n" + game_state["location"].description)
    choice = input("What do you want to do? (search boxes/go back) ")

    if choice == "search boxes":
        print("You found nothing in the boxes!")
    elif choice == "go back":
        game_state["location"] = Hall()

    return game_state


if __name__ == "__main__":
    start_game()
