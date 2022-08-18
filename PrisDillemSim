import random
import copy

BETRAY = 1
DONT_BETRAY = 0

# CHANGE THESE 5 VARS TO WHATEVER YOU WANT

players = 16 # This has to be an even number
move_amount = 10
games_run = 50000
player_mutation_chance = 1
move_mutation_chance = 10

# If Player 1 and Player 2 Betray each other they each gain 1 point
# If either Player doesnt betray whilst the other does, the betrayer gains 3 points whilst the other gains 0 points
# If neither Player betrays they both gain 2 points
def playGame(player1_list, player2_list, move_index):
    if player1_list[move_index] == BETRAY and player2_list[move_index] == BETRAY:
        player1_list[-1]+=1
        player2_list[-1]+=1
    if player1_list[move_index] == DONT_BETRAY and player2_list[move_index] == DONT_BETRAY:
        player1_list[-1]+=2
        player2_list[-1]+=2
    if player1_list[move_index] == BETRAY and player2_list[move_index] == DONT_BETRAY:
        player1_list[-1]+=3
        player2_list[-1]+=0
    if player1_list[move_index] == DONT_BETRAY and player2_list[move_index] == BETRAY:
        player1_list[-1]+=0
        player2_list[-1]+=3

def splitList(a_list):
    half = len(a_list)//2
    return a_list[:half], a_list[half:]

# This function splits the player list into to equally sized randomly chosen lists
# Then plays every move in their moveset against each other adding up scores as per prev function
def playMove(player_list, round_number):
    group1, group2, = splitList(player_list)
    for list_index in range(int(len(player_list)/2)):
        playGame(group1[list_index], group2[list_index], round_number)

# Each list in the playerlist has a player_mutation_chance chance of being mutated
# If this chance happens then each move in this set has a move_mutation_chance chance
# Of flipping its value
def mutatePlayers(player_list):
    for moveset in player_list:
        if random.randint(1,player_mutation_chance) == 1:
            for move in range(len(moveset)-1): # -1 for the score value
                if random.randint(1,move_mutation_chance) == 1:
                    moveset[move] = 1 - moveset[move]

# As per name       
def resetScore(player_list):
    for moveset in player_list:
        moveset[-1] = 0
        
# Just using this for custom sort since I dont want to use lambda
def getScore(move_list):
    return move_list[-1]

# The empty lists start with score at 0 and all moves at not betraying
def setup():
    player_list = []
    for player_iter in range(players):
        dont_betray_list = []
        for move_num in range(move_amount):
            dont_betray_list.append(DONT_BETRAY)

        # Note we keep the score at the very end of the move list
        dont_betray_list.append(0)
        player_list.append(dont_betray_list)

    return player_list

def doSingleGame(player_list):
    random.shuffle(player_list)
    for move in range(move_amount):
        playMove(player_list, move)
        
    player_list.sort(key=getScore)

    # The new player list will be the top winning half of the game we just
    # played, plus a mutated version of the the top winning half of the game
    # we just played
    winners = copy.deepcopy(player_list[int(players/2):])
    
    mutated_winners = copy.deepcopy(winners)
    mutatePlayers(mutated_winners)
    
    player_list = winners + mutated_winners
    resetScore(player_list)
    return player_list

def doAnalysis(player_list):
    print("Players:\t\t"+str(players))
    print("Moves Per Player:\t"+str(move_amount))
    print("Games Ran:\t"+str(games_run))
    print("Player Mutation Chance:\t1/"+str(player_mutation_chance))
    print("Move Mutation Chance:\t1/"+str(move_mutation_chance))
    print()
    count = 1
    for movelist in player_list:    
        print("Player "+str(count)+":\t"+" ".join(map(str,movelist[:-1])))
        count+=1

    totalBetrays=0
    for player in player_list:
        for move in player:
            totalBetrays+=move
    print()  
    print("Betray Count:\t\t"+str(totalBetrays))
    print("Don't Betray Count:\t"+str(((players*move_amount)- totalBetrays)))


if __name__ == "__main__":

    player_list = setup()
    for game in range(games_run):
        player_list = doSingleGame(player_list)

    doAnalysis(player_list)



