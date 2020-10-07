import random
suits = ('Hearts', 'Diamonds', 'Spades', 'Clubs')
ranks = ('Two', 'Three', 'Four', 'Five', 'Six', 'Seven', 'Eight', 'Nine', 'Ten', 'Jack', 'Queen', 'King', 'Ace')
values = {'Two':2, 'Three':3, 'Four':4, 'Five':5, 'Six':6, 'Seven':7, 'Eight':8, 'Nine':9, 'Ten':10, 'Jack':10,
         'Queen':10, 'King':10, 'Ace':11}
playing = True

class Card:
    
    def __init__(self,suit,rank):
        self.suit=suit
        self.rank=rank
        
    
    def __str__(self):
        return self.rank+" of "+self.suit

class Deck:
    
    def __init__(self):
        self.deck = []  # start with an empty list
        for suit in suits:
            for rank in ranks:
                self.deck.append(Card(suit,rank))
    
    def __str__(self):
        all_cards=""
        for card in self.deck:
            all_cards+= "\n"+card.rank+ " of "+card.suit
        return "Cards in deck:\n"+all_cards
    
    def shuffle(self):
        random.shuffle(self.deck)
        
    def deal(self):
        return self.deck.pop()
        
 def take_bet(chips):
    
    while True:
        try:
            chips.bet = int(input('How many chips would you like to bet? '))
        except ValueError:
            print('Sorry, a bet must be an integer!')
        else:
            if chips.bet > chips.total:
                print("Sorry, your bet can't exceed",chips.total)
            else:
                break 
                
 def hit(deck,hand):
    hand.add_card(deck.deal())
    hand.adjust_for_ace()
   
 def hit_or_stand(deck,hand):
    global playing # to control an upcoming while loop
    while True:
        a=input("Do you want to hit : Y or N :")
        if a[0].upper()=="Y":
            hit(deck,hand)
        elif a[0].upper()=="N":
            print("Player stands. Dealer is playing.")
            playing=False
        else:
            print("Sorry! Please choose a correct option")
            continue  
        break
        
def show_some(player,dealer):
    print("\nDealer's Hand:")
    print(" <card hidden>")
    print('',dealer.cards[1])  
    print("\nPlayer's Hand:", *player.cards, sep='\n ')
    
def show_all(player,dealer):
    print("\nDealer's Hand:", *dealer.cards, sep='\n ')
    print("Dealer's Hand =",dealer.value)
    print("\nPlayer's Hand:", *player.cards, sep='\n ')
    print("Player's Hand =",player.value)
    
def replay():
    
    return input('Do you want to play again? Enter Yes or No: ').lower().startswith('y')
    
def player_busts(player,dealer,chips):
    print("\nPlayer busts!")
    chips.lose_bet()

def player_wins(player,dealer,chips):
    print("Player wins!")
    chips.win_bet()

def dealer_busts(player,dealer,chips):
    print("Dealer busts!")
    chips.win_bet()
    
def dealer_wins(player,dealer,chips):
    print("Dealer wins!")
    chips.lose_bet()
    
def push(player,dealer):
    print("Dealer and Player tie! It's a push.")
    
while True:
    # Print an opening statement
    print("Welcome to black Jack")
    
    # Create & shuffle the deck, deal two cards to each player
    game_deck=Deck()
    game_deck.shuffle()
    gameing_player = Hand()
    gameing_player.add_card(game_deck.deal())
    gameing_player.add_card(game_deck.deal())
    dealer_player = Hand()
    dealer_player.add_card(game_deck.deal())
    dealer_player.add_card(game_deck.deal())
    
        
    # Set up the Player's chips
    player_chips=Chips()
    
    # Prompt the Player for their bet
    take_bet(player_chips)
    
    # Show cards (but keep one dealer card hidden)
    show_some(gameing_player,dealer_player)
    
    while playing:  # recall this variable from our hit_or_stand function
        
        # Prompt for Player to Hit or Stand
        hit_or_stand(game_deck,gameing_player)
        
        # Show cards (but keep one dealer card hidden)
        show_some(gameing_player,dealer_player)
        
        # If player's hand exceeds 21, run player_busts() and break out of loop
        if gameing_player.value>21:
            player_busts(gameing_player,dealer_player,player_chips)
            

    # If Player hasn't busted, play Dealer's hand until Dealer reaches 17
    if gameing_player.value <= 21:
        while dealer_player.value<17:
        
            hit(game_deck,dealer_player)
        
        # Show all cards
        show_all(gameing_player,dealer_player)
        # Run different winning scenarios
        if dealer_player.value>21:
            dealery_busts(gameing_player,dealer_player,player_chips)
        elif gameing_player.value<dealer_player.value:
            dealer_wins(gameing_player,dealer_player,player_chips)
        elif gameing_player.value>dealer_player.value:
            player_wins(gameing_player,dealer_player,player_chips)
        else:
            push(gameing_player,dealer_player)   
    
    # Inform Player of their chips total 
    print(f"Total Chips of player :{player_chips.total}")
    # Ask to play again
    if replay()==True:
        playing=True
        continue
    else:
        print("Thanks for playing!")
        break
