# apichallenge
Riot API Challenge repository for Stoxastic

The goal of this API challenge submissions is to use URF match data to create an application to 
determine the win percentage of one particular team composition against another for Ultra Rapid Fire (URF)
mode games. This project will collect match ids from the URF endpoint and use those to query match data
from the Riot Games API. Once a large amount of data is collected, individual champion win percentages 
will be determined and saved into a parsable file. An html based calculator will use this file to simulate
matches using individual champion win rates. Doing this many times will allow the calculator to come up with
an estimated win percentage for any team composition against another. 

The finished web page is being hosted and shared on Google Docs in the following URL:
https://020bb04d762759524d2aa1eab3b20f2ca13d6e9f-www.googledrive.com/host/0B_KZ5lOBsLqKfjdEdzlTeWdfbkd4WjQ5SUdJaVNWVVJ2cVJFdzJOT0hzbXB5UEtoNjdyNEk/wincalc.html

The following html files are used in this project:

--------------------------------------------------------------------------------------------------------------
# champions.html

This file queries the static data of the Riot API to get all of the champions in the game and their ID.
This file will display each champion's name with their ID, separated by comma; then a semicolon is used
to separate  the next champion's name and ID:

Thresh,412;Aatrox,266;Tryndamere,23; ... etc
This output is saved to champions.txt

--------------------------------------------------------------------------------------------------------------
# matchids.html

This file queries the new URF endpoint and retrieves a large list of match ids that the endpoint provides. 
This file will then output all of the matchids, separated by comma, to the browser:

1780443073,1780428241,1780404355,1780409401, ... etc
This output is saved to na_ids.txt. For this project only match ids from the NA server was used.

--------------------------------------------------------------------------------------------------------------
# winpcts.html

This file takes the file generated by matchids.html (na_ids.txt) and queries the Riot API for the match data.
The key points of information necessary are 

1) Champions used for each team
2) Which team won

With this information coming from thousands of URF matches, it's possible to determine the win percentage
of one champion versus another. Because there are so many champions, a large number of matches must be 
looked at because of the large number of possible matchups. 

Once all matches are retrieved and analyzed, the browser will output the aggregated win percentages of 
each champion matchup in the following format:

championA's id, championB's id, championA's win % vs championB, # games of championA vs championB;

All possible matchups are then available with their associated win percentages:

1,1,0.5,402;1,2,0.5119047619047619,84;1,3,0.5416666666666666,144; ... etc
This output is saved to na_pcts.txt

------------------------------------------------------------------------------------------------------------
# wincalc.html
This file will take the files champions.txt and win_pcts.txt to create a team composition win rate 
calculator. When run in a browser, it will take input from the user in autocomplete fields from the user. 
The user will select 5 champions on each team. Once the "Compute Win Percentage" button is pressed, the 
calculator will take all of individual win percentages in the matchup and simulate 100000 matches between 
the two team compositions. After the matches are finished simulating, the win percentage can be determined. 

There are 5 champions for each team, so there are 5 x 5 = 25 individual champions matchups, and therefore 
25 individual champion win percentages. Using a random number generator, each of these 25 win percentages
will be used to retrieve a score of 1 or 0, whether the probability of scoring a 1 is equal to the
individual win percentage. If the total score of a simulation is 13 or greater, the calculator considers it
a win. This simulation is ran 100000 times in order to reduce the variance of the random number generator.
The final win percentage is calculated be the number of wins divided by 100000. This win percentage is 
then displayed in the browser. 