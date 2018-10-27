# BlackJack-OOP
# BlackJack-OOP
from random import shuffle
###Sets the deck
def set_carddeck():
    ranks = ('Two', 'Three', 'Four', 'Five', 'Six', 'Seven', 'Eight', 'Nine', 'Ten', 'Jack', 'Queen', 'King', 'Ace')
    deck=["Hearts"+ " " + x for x in ranks]+["Diamonds"+ " " + x for x in ranks]+["Spades"+ " " + x for x in ranks]+["Clubs"+ " " + x for x in ranks]
    dict_deck_values={}
    NUM=74
    counter=1
    tencount=0
    totcount=-1
    while True:
        try:
            for i in range(NUM):
                if tencount==5:
                    tencount=0
                    counter=1
                    continue
                elif counter==10:
                    counter-=1
                    tencount+=1
                    continue
                else:
                    if totcount==11 or totcount==24 or totcount==37 or totcount==50:
                        totcount+=1
                        counter+=1
                        dict_deck_values[deck[totcount]]=11
                        continue
                    else:
                        totcount+=1
                        counter+=1
                        dict_deck_values[deck[totcount]]=counter
                        continue
        except:
            break
        result=[x for x in dict_deck_values.items()]
        shuffle(result)
        return result

### bet and fund managing system
def set_budget():
    global budget
    budget=int(input("what is your budget?: "))
def set_betamount():
    global bet
    bet=int(input("how much do you want to bet?"))
#initial Player hand                
def set_hand():
    global deck_count
    global gameon
    shuffled_deck
    my_hand.append(shuffled_deck[0])
    shuffled_deck.pop(0)
    my_hand_worth.append(my_hand[-1][1])
    my_hand.append(shuffled_deck[0])
    shuffled_deck.pop(0)
    my_hand_worth.append(my_hand[-1][1])
    deck_count-=2
    ace_joker()
    print(f"PLAYER cards: {[i[0] for i in my_hand]}\nTotal hand: {sum(my_hand_worth)}")
    if sum(my_hand_worth)==21:
            print("BLACKJACK!")
            gameon=False
    elif 9 in my_hand_worth or 10 in my_hand_worth or 11 in my_hand_worth or 1 in my_hand_worth:
        nine_ten_eleven()

#initial dealer hand
def set_dealer_hand():
    global deck_count
    shuffled_deck
    dealer_hand.append(shuffled_deck[0])
    shuffled_deck.pop(0)
    dealer_hand_worth.append(dealer_hand[-1][1])
    dealer_hand.append(shuffled_deck[0])
    shuffled_deck.pop(0)
    dealer_hand_worth.append(dealer_hand[-1][1])
    deck_count-=2
    ace_joker_dealer()
    print(f"DEALER cards: -MISTERY CARD- {dealer_hand[0][0]} \nTotal hand at least: {dealer_hand_worth[0]}")
#Boolean for drawing another card or dealer turn in case Stand        
def drawcard():
    another_card=input("Hit, Stand, Quit:").lower()[0]
    if another_card=="h":
        ace_joker()
        return True
    elif another_card=="s":
        dealer_turn()
        return False
    False
#Dealer Turn
def dealer_turn(): #if player stands
    #if sum(my_hand_worth)>21:
    #    print("Over 21, You Lose!")
    #    gameon==False
    if sum(my_hand_worth)==sum(dealer_hand_worth):
        pass
    else:
        while True:
            if sum(dealer_hand_worth)==21:
                print("You Lose, Dealer BlackJack!")
                gameon==False
                break
            elif sum(dealer_hand_worth)>sum(my_hand_worth):
                print(f"DEALER cards: {[i[0] for i in dealer_hand]}\nTotal hand: {sum(dealer_hand_worth)}")
                gameon==False
                break
            else:
                while (sum(dealer_hand_worth)==17 and "Ace" in [i[0][-3:] for i in dealer_hand]) or (sum(dealer_hand_worth)<17):
                    global deck_count
                    dealer_hand.append(shuffled_deck[0])
                    shuffled_deck.pop(0)
                    dealer_hand_worth.append(dealer_hand[-1][1])
                    deck_count-=1
                    ace_joker_dealer()
                #wingame()
                #print("no1")
                break
        #wingame()
        #print("no2")
#My turn    
def hand():
    global deck_count
    global gameon
    global one_more_card
    while one_more_card<1001 and drawcard() and deck_count>0 and sum(my_hand_worth)<22 and gameon:
        clear_output()
        my_hand.append(shuffled_deck[0])
        shuffled_deck.pop(0)
        my_hand_worth.append(my_hand[-1][1])
        one_more_card+=1
        deck_count-=1
        ace_joker()
        print(f"Dealer cards: -MISTERY CARD- {dealer_hand[0]} \nTotal hand at least: {dealer_hand_worth[0]}")
        if sum(my_hand_worth)>21:
            print(f"Card taken: {my_hand[-1]}\nPLAYER cards: {[i[0] for i in my_hand]}\nTotal hand: {sum(my_hand_worth)}")
            print("Over 21! you lose!")
            break
        elif sum(my_hand_worth)==21:
            #wingame()
            gameon=False
            break
        print(f"Card taken: {my_hand[-1]}\nPLAYER cards: {[i[0] for i in my_hand]}\nTotal hand: {sum(my_hand_worth)}") 
    
    #wingame()
    #print("no3")
    
#turns player ace into 1 if necessary    
def ace_joker():
    for i in range(0,len(my_hand)):
        if my_hand[i][0][-3:]=='Ace':
            if sum(my_hand_worth)>21:
                #my_hand[i][0],my_hand[i][1]=my_hand[i][0],1#this might be trouble
                my_hand_worth[i]=1
                
#turns dealer ace into 1 if necessary
def ace_joker_dealer():
    for i in range(0,len(dealer_hand)):
        if dealer_hand[i][0][-3:]=='Ace':
            if sum(dealer_hand_worth)>21:
                dealer_hand_worth[i]=1
def nine_ten_eleven():
    global bet
    global one_more_card
    Double=input("do you want to double your bet?:").lower()
    if Double[0]=="y":
        bet=bet*2
        one_more_card+=1000
    else:
        bet=bet
        one_more_card=0
        
#check if won    
def wingame():
    global budget
    if sum(my_hand_worth)==sum(dealer_hand_worth)==21 and len(dealer_hand_worth)==2:
        budget-=(bet+bet*1.5)
        print("\n")
        print("You lose, Dealer Blackjack!")
        print(f"Dealer cards: {[i[0] for i in dealer_hand]} Total Value: {sum(dealer_hand_worth)}")
        print(f"Player cards: {[i[0] for i in my_hand]} Total Value: {sum(my_hand_worth)}") 
        print(f"\nYou lost {bet+bet*1.5}, your budget is {budget}")
    elif sum(my_hand_worth)==sum(dealer_hand_worth):
        print("\n")
        print("PUSH")
        print(f"Dealer cards: {[i[0] for i in dealer_hand]} Total Value: {sum(dealer_hand_worth)}")
        print(f"Player cards: {[i[0] for i in my_hand]} Total Value: {sum(my_hand_worth)}")
        print(f"\nYour budget is {budget}")
    elif sum(dealer_hand_worth)==21 and len(dealer_hand_worth)==2 and (sum(my_hand_worth)<21 or sum(my_hand_worth)>21):
        budget-=(bet+bet*1.5)
        print("\n")
        print("You Lose, Dealer BLACKJACK!")
        print(f"Dealer cards: {[i[0] for i in dealer_hand]} Total Value: {sum(dealer_hand_worth)}")
        print(f"Player cards: {[i[0] for i in my_hand]} Total Value: {sum(my_hand_worth)}")
        print(f"\nYou lost {bet+bet*1.5}, your budget is {budget}")
    elif sum(my_hand_worth)==sum(dealer_hand_worth)==21 and len(my_hand_worth)==2:
        budget+=(bet+bet*1.5)
        print("\n")
        print("BLACKJACK!")
        print(f"Dealer cards: {[i[0] for i in dealer_hand]} Total Value: {sum(dealer_hand_worth)}")
        print(f"Player cards: {[i[0] for i in my_hand]} Total Value: {sum(my_hand_worth)}")    
        print(f"\nYou won {bet+bet*1.5}, your budget is {budget}")
    elif sum(my_hand_worth)>21:
        budget-=(bet+bet)
        print("\n")
        print("You lose, over 21!")
        print(f"Dealer cards: {[i[0] for i in dealer_hand]} Total Value: {sum(dealer_hand_worth)}")
        print(f"Player cards: {[i[0] for i in my_hand]} Total Value: {sum(my_hand_worth)}")
        print(f"\nYou lost {bet+bet}, your budget is {budget}")
    elif sum(dealer_hand_worth)==21 and sum(my_hand_worth)<21:
        budget-=(bet+bet)
        print("You Lose, Dealer TWENTY ONE!")
        print(f"Dealer cards: {[i[0] for i in dealer_hand]} Total Value: {sum(dealer_hand_worth)}")
        print(f"Player cards: {[i[0] for i in my_hand]} Total Value: {sum(my_hand_worth)}")
        print(f"\nYou lost {bet+bet}, your budget is {budget}")
    elif sum(my_hand_worth)>sum(dealer_hand_worth):
        budget+=(bet+bet)
        print("\n")
        print("You Win, more than dealer!")
        print(f"Dealer cards: {[i[0] for i in dealer_hand]} Total Value: {sum(dealer_hand_worth)}")
        print(f"Player cards: {[i[0] for i in my_hand]} Total Value: {sum(my_hand_worth)}")
        print(f"\nYou won {bet+bet}, your budget is {budget}")
    elif sum(dealer_hand_worth)>21:
        budget+=(bet+bet)
        print("\n")
        print("You Win, dealer went over 21!")
        print(f"Dealer cards: {[i[0] for i in dealer_hand]} Total Value: {sum(dealer_hand_worth)}")
        print(f"Player cards: {[i[0] for i in my_hand]} Total Value: {sum(my_hand_worth)}")
        print(f"\nYou won {bet+bet}, your budget is {budget}")
    elif sum(my_hand_worth)<sum(dealer_hand_worth) and sum(dealer_hand_worth)<22:
        budget-=(bet+bet)
        print("\n")
        print("You lose, less than dealer!")
        print(f"Dealer cards: {[i[0] for i in dealer_hand]} Total Value: {sum(dealer_hand_worth)}")
        print(f"Player cards: {[i[0] for i in my_hand]} Total Value: {sum(my_hand_worth)}")
        print(f"\nYou lost {bet+bet}, your budget is {budget}")
    else:
        print("\n")
        print("you have quit the game")
        print(f"Dealer cards: {[i[0] for i in dealer_hand]} Total Value: {sum(dealer_hand_worth)}")
        print(f"Player cards: {[i[0] for i in my_hand]} Total Value: {sum(my_hand_worth)}")
        print(f"\nYou lost {bet+bet}, your budget is {budget}")

from IPython.display import clear_output
#gameplay
budget=0
set_budget()
while True:
    #global budget
    set_carddeck()
    shuffled_deck=set_carddeck()
    deck_count=52
    print(f"Cards in deck: {len(shuffled_deck)}")
    used_deck=[]
    my_hand=[]
    my_hand_worth=[]
    dealer_hand=[]
    dealer_hand_worth=[]
    bet=0
    one_more_card=0
    gameon=True
    while gameon:
        set_betamount()
        set_dealer_hand()
        set_hand()
        if gameon==False:
            break
        hand()
        wingame()
        break
    again=input("Another game?: ")
    if again[0]=="y":
        continue
    else:
        print("Thank you for playing!")
        break
