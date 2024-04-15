import database

# the constant for maximum change 
K = 50

# Prediction of who will win. This makes winning and losing more important than anything else. 
def playerElo(playerA, playerB):
    eloA = 1 / (1 + 10**((playerB - playerA) / 400.0))
    eloB = 1 / (1 + 10**((playerA - playerB) / 400.0))
    return eloA, eloB

# Results from the game
def winLoss(A_score, B_score):
    pointDif = abs(A_score - B_score)
    Sa = 0
    Sb = 0
    
    if A_score > B_score:
        Sa = 1
        Sb = 0
    elif A_score < B_score:
        Sa = 0
        Sb = 1
        
    return Sa, Sb, pointDif

# 1v1 Ranking System: Change to the new ranking after playing a game (Should be in the backend. This should link to player profiles and games after they are played.) 
def afterGameRank():
    playerA_email = input("Enter Player A's email: ")
    playerA_info = database.query_player_by_email(playerA_email)
    if playerA_info is None:
        playerA_first_name = input("Enter Player A's first name: ")
        playerA_last_name = input("Enter Player A's last name: ")
        playerA_info = {"email": playerA_email, "first_name": playerA_first_name, "last_name": playerA_last_name, "rating": 1000.0}
        database.insert_player(playerA_info)
    
    playerB_email = input("Enter Player B's email: ")
    playerB_info = database.query_player_by_email(playerB_email)
    if playerB_info is None:
        playerB_first_name = input("Enter Player B's first name: ")
        playerB_last_name = input("Enter Player B's last name: ")
        playerB_info = {"email": playerB_email, "first_name": playerB_first_name, "last_name": playerB_last_name, "rating": 1000.0}
        database.insert_player(playerB_info)

    pEloA, pEloB = playerElo(playerA_info["rating"], playerB_info["rating"])
    
    A_score = int(input("Enter Player A's score: "))
    B_score = int(input("Enter Player B's score: "))
    Sa, Sb, x = winLoss(A_score, B_score)
    
    playerA_rating = playerA_info["rating"] + K * (Sa - pEloA)
    playerB_rating = playerB_info["rating"] + K * (Sb - pEloB)
    
    if Sa == 1:
        playerA_rating += x
        playerB_rating -= x
    elif Sa == 0:
        playerA_rating -= x
        playerB_rating += x

    # Update player information in the database (still working to understand how the player data should be stored, managed, and displayed) 
    database.update_player_rating(playerA_email, playerA_rating)
    database.update_player_rating(playerB_email, playerB_rating)
    
    return playerA_rating, playerB_rating

# Retrieve top 10 ranked players based on Elo ratings (This will show the top players on the web and idealy will be realtime. 
def rankedPlayers():
    sorted_players = database.query_top_players(10)
    print(sorted_players)
    return sorted_players
