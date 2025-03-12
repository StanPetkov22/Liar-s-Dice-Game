import random

def roll_dice(num_dice):
    return [random.randint(1, 6) for _ in range(num_dice)]

def count_dice(dice_list):
    counts = {i: 0 for i in range(1, 7)}
    for die in dice_list:
        counts[die] += 1
    return counts

def get_player_bid():
    while True:
        try:
            quantity = int(input("Enter the number of dice: "))
            face = int(input("Enter a face value (1-6): "))

            if quantity < 1 or face < 1 or face > 6:
                print("Error: Please enter valid numbers (1-6 for face, positive number for quantity).")
                continue

            return (quantity, face)

        except ValueError:
            print("Error: Please enter numbers, not text.")

def computer_decision(current_bid, computer_dice, total_dice):
    quantity, face = current_bid
    all_counts = count_dice(computer_dice)

    known_count = all_counts[face]
    unknown_dice = total_dice - len(computer_dice)
    expected_extra = unknown_dice / 6
    total_expected = known_count + expected_extra

    confidence_threshold = 1.8
    bluff_factor = 0.2

    if total_expected * confidence_threshold < quantity:
        if random.random() < 0.6:
            print("Computer calls 'Liar'!")
            return "liar"

    if random.random() < bluff_factor:
        new_quantity = quantity + random.randint(1, 2)
        new_face = random.choice([i for i in range(1, 7) if i != face])
        print(f"Computer makes a bold bid: {new_quantity} of {new_face}")
        return (new_quantity, new_face)

    if total_expected >= quantity:
        new_quantity = quantity + 1
        new_face = face if random.random() < 0.8 else random.choice([i for i in range(1, 7)])
        print(f"Computer raises: {new_quantity} of {new_face}")
        return (new_quantity, new_face)

    print("Computer calls 'Liar'!")
    return "liar"

def check_bid(current_bid, player_dice, computer_dice):
    quantity, face = current_bid
    all_dice = player_dice + computer_dice
    count = count_dice(all_dice)

    return count[face] >= quantity

def liars_dice():
    player_dice = 5
    computer_dice = 5
    current_bid = None

    while player_dice > 0 and computer_dice > 0:
        print("\n--- New Round ---")
        player_hand = roll_dice(player_dice)
        computer_hand = roll_dice(computer_dice)

        print(f"Your dice: {player_hand}")

        if not current_bid:
            current_bid = get_player_bid()
        else:
            action = input("Will you raise (bid) or call 'Liar'?: ").strip().lower()
            if action == "liar":
                if check_bid(current_bid, player_hand, computer_hand):
                    print("The bid was correct! You lose one die.")
                    player_dice -= 1
                else:
                    print("The bid was incorrect! The computer loses one die.")
                    computer_dice -= 1
                current_bid = None
                continue
            else:
                current_bid = get_player_bid()

        computer_move = computer_decision(current_bid, computer_hand, player_dice + computer_dice)
        if computer_move == "liar":
            if check_bid(current_bid, player_hand, computer_hand):
                print("The computer challenged incorrectly! It loses one die.")
                computer_dice -= 1
            else:
                print("The computer was right! You lose one die.")
                player_dice -= 1
            current_bid = None
        else:
            current_bid = computer_move

    if player_dice == 0:
        print("Computer Wins!")
    else:
        print("You Win!")

liars_dice()
