#this is zander hi
import random

# Player attributes
player = {
    "health": 100,
    "inventory": [],
}

# Enemy types with different behaviors
enemies = {
    "Goblin": {"health": 30, "attack": 10, "behavior": "aggressive"},
    "Thief": {"health": 25, "attack": 7, "behavior": "trickster"},
}

# Random encounter generator
def random_encounter():
    if random.choice([True, False]):  # 50% chance of encounter
        enemy = random.choice(list(enemies.keys()))
        print(f"\nYou encounter a {enemy}!")
        fight_enemy(enemy)

# Combat system
def fight_enemy(enemy_name):
    enemy = enemies[enemy_name]
    while player["health"] > 0 and enemy["health"] > 0:
        action = input("\nChoose an action: (1) Attack (2) Run: ")
        if action == "1":
            damage = random.randint(5, 15)
            enemy["health"] -= damage
            print(f"You attack the {enemy_name} for {damage} damage!")
            if enemy["health"] <= 0:
                print(f"You defeated the {enemy_name}!")
                return
            # Enemy attacks back
            player["health"] -= enemy["attack"]
            print(f"The {enemy_name} attacks you for {enemy['attack']} damage!")
        elif action == "2":
            if random.choice([True, False]):  # 50% escape chance
                print("You managed to escape!")
                return
            else:
                print("You failed to escape!")
        else:
            print("Invalid action.")

    if player["health"] <= 0:
        print("\nYou have been defeated. Game over!")
        exit()

# Main game loop
def explore():
    while True:
        print("\nYou are at a crossroads.")
        print("1. Enter the Labyrinth")
        print("2. Explore the ruins")
        print("3. Check inventory")
        print("4. Quit game")

        choice = input("\nWhat do you want to do? ")

        if choice == "1":
            print("\nYou step into the Labyrinth...")
            random_encounter()
        elif choice == "2":
            print("\nYou enter the ruins and find an old sword!")
            player["inventory"].append("Old Sword")
        elif choice == "3":
            print("\nYour inventory:", player["inventory"])
        elif choice == "4":
            print("\nThanks for playing!")
            break
        else:
            print("\nInvalid choice.")

# Start the game
print("Welcome to the Text Adventure Game!")
explore()
