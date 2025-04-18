Okay, here is the updated documentation incorporating the log events identified in the Javascript file, presented in the requested Markdown style:

# Log Library

## General Log Syntax:

Every line in the log follows this format:

```
DD/MM/YYYY - HH:MM:SS.mmm - [Log Statement]
```

For example:

```
04/12/2025 - 15:03:02.094 - Starting Freeze period
```

<details>
<summary><h3>Player Information</h3></summary>

If a log entry involves a player, the player's information is typically formatted as follows:

```
"[Player Name]\<[Player ID]\>\<[Player SteamID3]\>\<[Player Side]\>"
```

For example:

```
"M J P\<2\>\<[U:1:395318202]\>\<CT\>"
```

**Player Name**
The Steam profile name of the player. This can include spaces and special characters.

**Player ID**
A server-specific identifier that can change each time a player reconnects. While useful within the `status` command, it's not a reliable permanent identifier.

**Player SteamID3**
The unique and consistent SteamID3 of the player's Steam profile. This is the recommended way to permanently identify a player. You can find more information at [https://developer.valvesoftware.com/wiki/SteamID](https://developer.valvesoftware.com/wiki/SteamID). Although not directly shown in the `status` command, it's available within the `JSON_BEGIN` block. For bots, the SteamID3 is simply `BOT`, as seen in this example: `"Arno<0><BOT><TERRORIST>"`.

**Player Side**
Indicates the player's current team: `CT` (Counter-Terrorist), `TERRORIST`, `Unassigned`, or `Spectator`.

</details>

### Match Events

Log entries related to match events begin with `MatchStatus:`.

For example:

```
04/12/2025 - 15:03:02.094 - MatchStatus: Score: 0:0 on map "de\_mirage" RoundsPlayed: 0
```

<details>
<summary><h4>Team Events</h4></summary>

Log entries related to team scoring and status updates.

**Team Scored**
Logs when a team wins a round, updating their score.

```
Team "[Side]" scored "[Score]" with "[Players]" players
```

Example:

```
04/12/2025 - 15:04:30.500 - Team "CT" scored "1" with "4" players
```

  * `[Side]`: The team that scored (`CT` or `TERRORIST`).
  * `[Score]`: The new total score for that team.
  * `[Players]`: The number of players remaining on the scoring team when the round ended.

**Team Notice**
Provides specific win/loss condition information for a round, along with the scores at that moment.

```
Team "[Side]" triggered "[Notice Message]" (CT "[CT Score]") (T "[T Score]")
```

Example:

```
04/12/2025 - 15:04:30.500 - Team "CT" triggered "SFUI_Notice_CTs_Win" (CT "1") (T "0")
```

  * `[Side]`: The team associated with the notice (often the winning team).
  * `[Notice Message]`: A game-internal message code describing the round outcome (e.g., `SFUI_Notice_CTs_Win`, `SFUI_Notice_Terrorists_Win`, `SFUI_Notice_Target_Bombed`, `SFUI_Notice_Bomb_Defused`).
  * `[CT Score]`: The Counter-Terrorist score at the time of the notice.
  * `[T Score]`: The Terrorist score at the time of the notice.

</details>

### World Events

Events triggered by the game world start with ` World triggered  `.

<details>
<summary><h4>World Event Details</h4></summary>

Log entries detailing events triggered by the game world itself.

**Match Start**
Indicates the beginning of the match on a specific map.

```
World triggered "Match_Start" on "[Map Name]"
```

Example:

```
04/12/2025 - 15:00:00.000 - World triggered "Match_Start" on "de_mirage"
```

  * `[Map Name]`: The name of the map being played (e.g., `de_mirage`).

**Round Start**
Indicates the start of a new round.

```
World triggered "Round_Start"
```

Example:

```
04/12/2025 - 15:03:02.094 - World triggered "Round_Start"
```

**Round End**
Indicates the end of a round.

```
World triggered "Round_End"
```

Example:

```
04/12/2025 - 15:04:30.500 - World triggered "Round_End"
```

**Round Restart**
Indicates the server is restarting the current round after a specified delay.

```
World triggered "Restart_Round_([Seconds]_second)"
```

Example:

```
04/12/2025 - 15:10:05.100 - World triggered "Restart_Round_(5_second)"
```

  * `[Seconds]`: The number of seconds until the round restarts.

**Game Commencing**
Typically signals the end of the warm-up phase and the start of the actual game.

```
World triggered "Game_Commencing"
```

Example:

```
04/12/2025 - 15:02:55.000 - World triggered "Game_Commencing"
```

</details>

<details>
<summary><h3>Player Connection & Status</h3></summary>

Log entries related to players joining, leaving, or changing status.

**Player Connected**
Logged when a player initially connects to the server.

```
"[Player Info]<>" connected, address "[IP Address]"
```

Example:

```
04/12/2025 - 14:59:10.123 - "New Player<3><[U:1:123456789]><>" connected, address "192.168.1.100:27015"
```

  * `[Player Info]`: Standard player identification string (Name, ID, SteamID3). Note the side is empty initially.
  * `[IP Address]`: The IP address and port the player connected from.

**Player Entered Game**
Logged when a connected player fully enters the playable game world.

```
"[Player Info]<>" entered the game
```

Example:

```
04/12/2025 - 14:59:15.456 - "New Player<3><[U:1:123456789]><>" entered the game
```

  * `[Player Info]`: Standard player identification string (Name, ID, SteamID3).

**Player Disconnected**
Logged when a player leaves the server.

```
"[Player Info]" disconnected (reason "[Reason]")
```

Example:

```
04/12/2025 - 15:30:00.987 - "M J P<2><[U:1:395318202]><CT>" disconnected (reason "Disconnect by user.")
```

  * `[Player Info]`: Standard player identification string, including their last known side.
  * `[Reason]`: The reason provided for the disconnection.

**Player Switched Team**
Logged when a player changes teams (including to Spectator or Unassigned).

```
"[Player Name]<[Player ID]><[Player SteamID3]>" switched from team <[Old Side]> to <[New Side]>
```

Example:

```
04/12/2025 - 15:01:05.678 - "M J P<2><[U:1:395318202]>" switched from team <Unassigned> to <CT>
```

  * `[Player Name]`, `[Player ID]`, `[Player SteamID3]`: Player identifiers.
  * `[Old Side]`: The player's previous team (`CT`, `TERRORIST`, `Spectator`, `Unassigned`).
  * `[New Side]`: The player's new team.

**Player Banned**
Logged when a player is banned from the server.

```
Banid: "[Player Info]" was banned "[Duration]" by "[Banner]"
```

Example:

```
04/12/2025 - 15:25:10.345 - Banid: "SuspiciousPlayer<4><[U:1:987654321]><TERRORIST>" was banned "permanently" by "Console"
```

  * `[Player Info]`: Standard player identification string.
  * `[Duration]`: The duration of the ban (e.g., `permanently`, `60 minutes`).
  * `[Banner]`: Who initiated the ban (e.g., `Console`, an admin's name).

</details>

<details>
<summary><h3>Player Actions</h3></summary>

Log entries detailing actions performed by players in the game.

**Player Chat Message**
Logs messages sent by players to either team chat or all chat. Excludes messages starting with command prefixes (`!`, `.`, `/`).

```
"[Player Info]" say "Message Text"
"[Player Info]" say_team "Message Text"
```

Example (All Chat):

```
04/12/2025 - 15:05:00.111 - "Player1<2><[U:1:12345]><CT>" say "gl hf"
```

Example (Team Chat):

```
04/12/2025 - 15:05:05.222 - "Player2<6><[U:1:67890]><TERRORIST>" say_team "Rush B"
```

  * `[Player Info]`: Standard player identification string.
  * `say` / `say_team`: Indicates all chat or team-only chat.
  * `[Message Text]`: The content of the chat message.

**Player Chat Command**
Logs chat messages identified as commands (starting with `!`, `.`, or `/`).

```
"[Player Info]" say "[Prefix][Command] [Arguments...]"
"[Player Info]" say_team "[Prefix][Command] [Arguments...]"
```

Example (All Chat):

```
04/12/2025 - 15:12:34.567 - "Admin<1><[U:1:11111]><Spectator>" say "!slay Player2"
```

Example (Team Chat):

```
04/12/2025 - 15:13:00.999 - "Player1<2><[U:1:12345]><CT>" say_team ".drop"
```

  * `[Player Info]`: Standard player identification string.
  * `say` / `say_team`: Indicates all chat or team-only chat.
  * `[Prefix]`: The command prefix used (`!`, `.`, or `/`).
  * `[Command]`: The command word itself.
  * `[Arguments...]`: Any additional text following the command, treated as arguments.

**Player Purchased Item**
Logs when a player buys equipment.

```
"[Player Info]" purchased "[Item Name]"
```

Example:

```
04/12/2025 - 15:03:05.300 - "M J P<2><[U:1:395318202]><CT>" purchased "m4a1"
```

  * `[Player Info]`: Standard player identification string.
  * `[Item Name]`: The internal name of the purchased item (e.g., `ak47`, `vesthelm`, `flashbang`).

**Player Picked Up Item**
Logs when a player acquires an item from the ground.

```
"[Player Info]" picked up "[Item Name]"
```

Example:

```
04/12/2025 - 15:15:20.400 - "Player2<6><[U:1:67890]><TERRORIST>" picked up "c4"
```

  * `[Player Info]`: Standard player identification string.
  * `[Item Name]`: The internal name of the item picked up.

**Player Dropped Item**
Logs when a player drops an item.

```
"[Player Info]" dropped "[Item Name]"
```

Example:

```
04/12/2025 - 15:16:30.500 - "Player1<2><[U:1:12345]><CT>" dropped "awp"
```

  * `[Player Info]`: Standard player identification string.
  * `[Item Name]`: The internal name of the item dropped.

</details>

<details>
<summary><h3>Game State Changes</h3></summary>

Log entries related to the overall state of the match or round.

**Freeze Time Start**
Indicates the beginning of the pre-round period where players cannot move.

```
Starting Freeze period
```

Example:

```
04/12/2025 - 15:03:02.094 - Starting Freeze period
```

**Game Over**
Logs the final result of the match.

```
Game Over: [Game Mode] [Sub Mode] [Map] score [CT Score]:[T Score] after [Duration] min
```

Example:

```
04/12/2025 - 15:48:00.000 - Game Over: competitive none de_mirage score 16:12 after 45 min
```

  * `[Game Mode]`: The primary game mode (e.g., `competitive`).
  * `[Sub Mode]`: Often `none`, might specify variations in other modes.
  * `[Map]`: The map the game was played on.
  * `[CT Score]`: Final score for the Counter-Terrorist team.
  * `[T Score]`: Final score for the Terrorist team.
  * `[Duration]`: Total duration of the match in minutes.

</details>

<details>
<summary><h3>JSON Data</h3></summary>

At the beginning of each round, a JSON object containing round statistics is logged.

```json
{
  "name": "round_stats",
  "round_number" : "1",
  "score_t" : "0",
  "score_ct" : "0",
  "map" : "de_mirage",
  "server" : "Counter-Strike 2",
  "fields" : "accountid, team, money, kills, deaths, assists, dmg, hsp, kdr, adr, mvp, ef, ud, 3k, 4k, 5k, clutchk, firstk, pistolk, sniperk, blindk, bombk, firedmg, uniquek, dinks, chickenk",
  "players" : {
    "player_0" : "0, 2, 800, 0, 0, 0, 0, 0.00, 0.00, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0",
    "player_2" : "395318202, 3, 800, 0, 0, 0, 0, 0.00, 0.00, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0"
  }
}
```

**round\_number**
The number of the current round being played.

**score\_t**
The current score of the TERRORIST side.

**score\_ct**
The current score of the Counter-Terrorist side.

**map**
The identifier of the map the match is taking place on.

**server**
The hostname of the game server.

<details>
<summary><h4>Fields and Player Data</h4></summary>

The `"fields"` key and the strings within the `"players"` array together form a table of player statistics for the current round. The `"fields"` string lists the column headers for this data.

| Field       | Description                                                                                                                                                                                             |
|-------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **accountid** | A shortened numerical representation of the SteamID3 (e.g., `[U:1:395318202]` becomes `395318202`). While it can be converted back, the conversion isn't always guaranteed.                               |
| **team** | The player's team, represented numerically (e.g., `2` for Terrorists, `3` for Counter-Terrorists).                                                                                                      |
| **money** | The amount of in-game currency the player currently has.                                                                                                                                              |
| **kills** | The total number of opponents the player has eliminated in the current match.                                                                                                                          |
| **deaths** | The total number of times the player has been eliminated in the current match.                                                                                                                          |
| **assists** | The number of times the player has assisted a teammate in getting a kill, typically by dealing significant damage.                                                                                       |
| **dmg** | Damage Per Round (DPR) or Total Damage Dealt. The total health points of damage the player has inflicted on enemies.                                                                                    |
| **hsp** | Headshot Percentage: (Headshot Kills / Total Kills) \* 100. The percentage of the player's kills that were headshots.                                                        *Float value.*                  |
| **kdr** | Kill/Death Ratio: kills divided by deaths. If the player has zero deaths, this is represented as the total number of kills.                                                                            *Float value.* |
| **adr** | Average Damage per Round: total damage dealt divided by the number of rounds the player has played.                                                                                                    *Float value.* |
| **mvp** | The number of times the player has been the Most Valuable Player of a round, based on impactful actions.                                                                                               |
| **ef** | Enemies Flashed: The number of opponents successfully blinded by the player's flashbang grenades.                                                                                                      |
| **ud** | Utility Damage: The total damage dealt to enemies using utility grenades (HE Grenades, Molotovs, Incendiary Grenades).                                                                                   |
| **3k** | The number of rounds in which the player achieved exactly 3 kills.                                                                                                                                     |
| **4k** | The number of rounds in which the player achieved exactly 4 kills.                                                                                                                                     |
| **5k** | The number of rounds in which the player achieved 5 kills (an "ace").                                                                                                                                  |
| **clutchk** | Clutch Kills: The number of kills the player secured while being the last player alive on their team against one or more opponents. This often counts total kills within successful clutch rounds won. |
| **firstk** | First Kills / Opening Kills: The number of rounds in which the player secured the first kill.                                                                                                         |
| **pistolk** | Pistol Kills: The total number of kills achieved using pistols.                                                                                                                                         |
| **sniperk** | Sniper Kills: The total number of kills achieved using sniper rifles (e.g., AWP, SSG 08, SCAR-20, G3SG1).                                        *Includes autosnipers.*                               |
| **blindk** | Blind Kills: The number of kills achieved while the player's vision was significantly impaired by a flashbang.                                                                                           |
| **bombk** | Kills directly resulting from a planted bomb explosion.                                                                                                                                               |
| **firedmg** | Fire Damage: The total damage dealt to enemies specifically by Molotov cocktails or Incendiary Grenades.                                                                                                |
| **uniquek** | The number of unique opponents the player has killed.                                                                                                                                                  |
| **dinks** | The number of times the player hit an enemy in the head without killing them.                                                                                                                          |
| **chickenk** | The total number of chickens killed by the player.                  *A very important statistic.*                                                                                            |

</details>

For example, a full JSON log entry might look like this:

```
04/12/2025 - 15:03:02.094 - JSON_BEGIN{
04/12/2025 - 15:03:02.094 - "name": "round_stats",
04/12/2025 - 15:03:02.094 - "round_number" : "1",
04/12/2025 - 15:03:02.094 - "score_t" : "0",
04/12/2025 - 15:03:02.094 - "score_ct" : "0",
04/12/2025 - 15:03:02.094 - "map" : "de_mirage",
04/12/2025 - 15:03:02.094 - "server" : "Counter-Strike 2",
04/12/2025 - 15:03:02.094 - "fields" : "accountid, team, money, kills, deaths, assists, dmg, hsp, kdr, adr, mvp, ef, ud, 3k, 4k, 5k, clutchk, firstk, pistolk, sniperk, blindk, bombk, firedmg, uniquek, dinks, chickenk",
04/12/2025 - 15:03:02.094 - "players" : {
04/12/2025 - 15:03:02.094 - "player_0" : "0, 2, 800, 0, 0, 0, 0, 0.00, 0.00, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0",
04/12/2025 - 15:03:02.094 - "player_2" : "395318202, 3, 800, 0, 0, 0, 0, 0.00, 0.00, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0"
04/12/2025 - 15:03:02.094 - }}JSON_END
```

</details>
