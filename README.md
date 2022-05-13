# cmsc320final

Final project and tutorial for CMSC320 at UMD

Looking at Fantasy Football data and trends from the 2021-2022 NFL season

wr["Player Name"] = list(map(lambda x: re.search("^.*\s.*(?=[A-Z]\.)", x).group(), wr["Player"]["Player"]))
