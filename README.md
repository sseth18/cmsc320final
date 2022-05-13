# cmsc320final

Final project and tutorial for CMSC320 at UMD

Looking at Fantasy Football data and trends from the 2021-2022 NFL season

def get_bad(df): #returns a list of the players in the bottom left corner
    meanTouches = np.mean(df["Touches"])
    meanPPT = np.mean(df["Points per Touch"])
    bad_players = []
    
    for ind,player in df.iterrows():
        if (player["Touches"]<meanTouches)[0] and (player["Points per Touch"]<meanPPT)[0]:
            bad_players.append(player["Player Name"][0])
        
    return bad_players
