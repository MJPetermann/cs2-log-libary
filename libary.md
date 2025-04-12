# Log Libary
## General Log Syntax:
Every line has the following format:

    DD/MM/YYYY - HH:MM:SS.mmm - [Log Statement]
For example:

    04/12/2025 - 15:03:02.094 - Starting Freeze period

### Player
If the log involves a player most of the time the player is formatted the following way: 

    "[Player Name]<[Player ID]><[Player SteamID3]><[Player Side]>"
    
For example:

    "M J P<2><[U:1:395318202]><CT>"
    
**Player Name** 
The Player name is the Steam profile name of the player. It can include spaces and special charakters.

**Player ID**
The Player ID is server specific and can/will change with each reconnect. It can be used as an identifier within the `status` command. Because of its inconsistancy it is not fit to be a permanent identifier!

**Player SteamID3**
The Player SteamID3 is the SteamID3 of the player. It is consistant for each Steam profile and therefore can be used to identify a player permanently. Further reference: https://developer.valvesoftware.com/wiki/SteamID
Though the SteamID3 isn't printed in the `status` command, it can be found in the JSON_BEGIN.
In case the Player is a bot the SteamID3 is just `BOT` as seen here `"Arno<0><BOT><TERRORIST>"`

**Player Side**
Can be either `CT`, `TERRORIST`, `Unassigned` or `Spectator`

### Match Events
Match events begin with `MatchStatus:`
For example

    04/12/2025 - 15:03:02.094 - MatchStatus: Score: 0:0 on map "de_mirage" RoundsPlayed: 0

### World Events
Some events are triggered by the world. They begin with `World triggered `
For example

    04/12/2025 - 15:03:02.094 - World triggered "Match_Start" on "de_mirage"

### JSON Data
At the start of every Round a JSON-Object is printed in the console.

    {
	    "name": "round_stats",
	    "round_number" : "1",
	    "score_t" : "0",
	    "score_ct" : "0",
	    "map" : "de_mirage",
	    "server" : "Counter-Strike 2",
	    "fields" : "             accountid,   team,  money,  kills, deaths,assists,    dmg,    hsp,    kdr,    adr,    mvp,     ef,     ud,     3k,     4k,     5k,clutchk, firstk,pistolk,sniperk, blindk,  bombk,firedmg,uniquek,  dinks,chickenk"
	    "players" : {
		    "player_0" : "                   0,      2,    800,      0,      0,      0,      0,   0.00,   0.00,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0"
		    "player_2" : "           395318202,      3,    800,      0,      0,      0,      0,   0.00,   0.00,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0"
    }}
**round_number**
This number describes the number of the current to be played round.

**score_t**
This represents the score of the TERRORIST-Side.

**score_ct**
This represents the score of the CT-Side.

**map**
This is the ID of the map the match is being played on.

**server**
This is the `hostname` of the server.



**fields** and the player strings in the **players** array
They together are an table with the following fields:

-----------

**accountid**  
The accountid is a short representation of the SteamID3. A SteamID3 like `[U:1:395318202]` will be `395318202`. It can be converted back by adding the missing parts, but the conversion is not guaranteed.

**team**  
The team the player belonges to. Represented numerically (e.g., `2` for Terrorists, `3` for Counter-Terrorists).

**money**  
The amount of in-game currency the player possesses.

**kills**  
The total number of opponents eliminated by the player.

**deaths**  
The total number of times the player was eliminated.

**assists**  
The number of times the player contributed to a teammate's kill, usually by dealing a significant amount of damage to the enemy.

**dmg**  
Damage Per Round (DPR) or Total Damage Dealt. The total amount of health points dealt by the player to enemy.

**hsp**  
Headshot Percentage. The percentage of the player's total kills that were achieved via headshots. Calculated as (Headshot Kills / Total Kills) * 100.

**kdr**  
Kill/Death Ratio. Calculated as kills divided by deaths. If deaths are zero, this is represented as the number of kills.

**adr**  
Average Damage per Round. Calculated as total dmg divided by the number of rounds played by the player.

**mvp**  
The number of times the player earned the Most Valuable Player (MVP) award for a round, based on impactful actions like kills, bomb plants/defuses, etc.

**ef**  
Enemies Flashed. The number of enemies successfully blinded by the player's flashbangs..

**ud**  
Utility Damage. The total amount of damage dealt to enemies using utility grenades (HE Grenades, Molotovs, Incendiary Grenades).

**3k**  
The number of rounds in which the player achieved exactly 3 kills.

**4k**  
The number of rounds in which the player achieved exactly 4 kills.

**5k**  
The number of rounds in which the player achieved 5 kills (an "ace").

**clutchk**  
Clutch Kills. The number of kills secured by the player while in a clutch situation (i.e., being the last player alive on their team against one or more opponents). This often counts the total kills within successful clutch rounds won by the player.

**firstk**  
First Kills / Opening Kills. The number of rounds where the player secured the first kill of the round.

**pistolk**  
Pistol Kills. The total number of kills achieved by the player using pistols.

**sniperk**  
Sniper Kills. The total number of kills achieved by the player using sniper rifles (e.g., AWP, SSG 08, SCAR-20, G3SG1).

**blindk**  
Blind Kills. The number of kills achieved by the player while their own vision was significantly impaired (usually fully white) by a flashbang.

**bombk**  
Bomb Kills.

**firedmg**  
Fire Damage. The total amount of damage dealt to enemies specifically by Molotov cocktails or Incendiary Grenades.

**uniquek**  
Unique Kills.

**dinks**  
The number of times the player landed a headshot on an enemy that did not result in a kill (leaving the opponent damaged).

**chickenk**  
Chicken Kills. The total number of chickens killed by the player.

For example:

    04/12/2025 - 15:03:02.094 - JSON_BEGIN{
    04/12/2025 - 15:03:02.094 - "name": "round_stats",
    04/12/2025 - 15:03:02.094 - "round_number" : "1",
    04/12/2025 - 15:03:02.094 - "score_t" : "0",
    04/12/2025 - 15:03:02.094 - "score_ct" : "0",
    04/12/2025 - 15:03:02.094 - "map" : "de_mirage",
    04/12/2025 - 15:03:02.094 - "server" : "Counter-Strike 2",
    04/12/2025 - 15:03:02.094 - "fields" : "             accountid,   team,  money,  kills, deaths,assists,    dmg,    hsp,    kdr,    adr,    mvp,     ef,     ud,     3k,     4k,     5k,clutchk, firstk,pistolk,sniperk, blindk,  bombk,firedmg,uniquek,  dinks,chickenk"
    04/12/2025 - 15:03:02.094 - "players" : {
    04/12/2025 - 15:03:02.094 - "player_0" : "                   0,      2,    800,      0,      0,      0,      0,   0.00,   0.00,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0"
    04/12/2025 - 15:03:02.094 - "player_2" : "           395318202,      3,    800,      0,      0,      0,      0,   0.00,   0.00,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0"
    04/12/2025 - 15:03:02.094 - }}JSON_END

