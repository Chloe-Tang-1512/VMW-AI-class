import random

# Player attributes
player = {
    "health": 100,
    "inventory": [],
    "xp": 0,
    "level": 1,
    "attack": 10
}

# Enemy types with different behaviors
enemies = {
    "Guard": {"health": 30, "attack": 10, "behavior": "aggressive"},
    "Assassin": {"health": 25, "attack": 15, "behavior": "stealthy"},
    "Brute": {"health": 40, "attack": 12, "behavior": "strong"}
}

# Leveling system
def level_up():
    if player["xp"] >= player["level"] * 15:
        player["level"] += 1
        player["health"] = min(100, player["health"] + 20)  # Restore some health on level up
        player["attack"] += 2  # Increase attack power
        print(f"\nCongratulations! You leveled up to Level {player['level']}! Your attack increased!")

# Healing function
def heal_player():
    if "Health Potion" in player["inventory"]:
        player["health"] = min(100, player["health"] + 30)
        player["inventory"].remove("Health Potion")
        print("\nYou drink a Health Potion and recover 30 HP!")
    else:
        print("\nYou don't have any healing items!")

# Combat system
def fight_enemy(enemy_name):
    enemy = enemies[enemy_name].copy()  # Prevent modifying the original enemy stats
    while player["health"] > 0 and enemy["health"] > 0:
        print(f"\n{enemy_name} Health: {enemy['health']} | Your Health: {player['health']}")
        action = input("\nChoose an action: (1) Light Attack (2) Heavy Attack (3) Dodge (4) Use Item (5) Run: ")

        if action == "1":
            damage = random.randint(player["attack"] - 3, player["attack"] + 3)
            enemy["health"] -= damage
            print(f"You deal {damage} damage to the {enemy_name}!")
        elif action == "2":
            if random.random() > 0.3:
                damage = random.randint(player["attack"], player["attack"] + 6)
                enemy["health"] -= damage
                print(f"Heavy hit! You deal {damage} damage to the {enemy_name}!")
            else:
                print("Your heavy attack missed!")
        elif action == "3":
            if random.random() > 0.5:
                print("You dodged the attack successfully!")
                continue
            else:
                print("You failed to dodge!")
        elif action == "4":
            heal_player()
            continue
        elif action == "5":
            if random.choice([True, False]):
                print("You successfully escaped!")
                return
            else:
                print("You failed to escape!")
        else:
            print("Invalid action.")
            continue

        if enemy["health"] <= 0:
            print(f"You defeated the {enemy_name}!")
            player["xp"] += 10
            level_up()
            return

        enemy_damage = random.randint(enemy["attack"] - 5, enemy["attack"] + 5)
        player["health"] -= enemy_damage
        print(f"The {enemy_name} attacks you for {enemy_damage} damage!")

        if player["health"] <= 0:
            print("\nYou have been defeated. Game over!")
            exit()

# Random encounter generator
def random_encounter():
    if random.choice([True, False]):
        enemy = random.choice(list(enemies.keys()))
        print(f"\nYou encounter a {enemy}!")
        fight_enemy(enemy)

# Main game loop
def explore():
    while True:
        print("\nYou are at a crossroad.")
        print("1. Enter the armoury")
        print("2. Explore the cell block")
        print("3. Visit the infirmary (Heal)")
        print("4. Check inventory")
        print("5. Quit game")

        choice = input("\nWhat do you want to do? ")

        if choice == "1":
            print("\nYou step into the armoury...")
            random_encounter()
        elif choice == "2":
            item_found = random.choice(["Knife", "Health Potion", "Gold Coin"])
            print(f"\nYou explore the cell block and find a {item_found}!")
            player["inventory"].append(item_found)
        elif choice == "3":
            print("\nYou visit the infirmary and find a Health Potion!")
            player["inventory"].append("Health Potion")
        elif choice == "4":
            print("\nYour inventory:", player["inventory"])
        elif choice == "5":
            print("\nThanks for playing!")
            break
        else:
            print("\nInvalid choice.")

# Start the game
print("Welcome to the Text Adventure Game!")
explore()
