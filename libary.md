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

### World Events

Events triggered by the game world start with `World triggered `.

For example:

```
04/12/2025 - 15:03:02.094 - World triggered "Match\_Start" on "de\_mirage"
````

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
````

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

| Field       | Description                                                                                                                                                                                             |
|-------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **accountid** | A shortened numerical representation of the SteamID3 (e.g., `[U:1:395318202]` becomes `395318202`). While it can be converted back, the conversion isn't always guaranteed.                               |
| **team** | The player's team, represented numerically (e.g., `2` for Terrorists, `3` for Counter-Terrorists).                                                                                                      |
| **money** | The amount of in-game currency the player currently has.                                                                                                                                              |
| **kills** | The total number of opponents the player has eliminated in the current match.                                                                                                                          |
| **deaths** | The total number of times the player has been eliminated in the current match.                                                                                                                          |
| **assists** | The number of times the player has assisted a teammate in getting a kill, typically by dealing significant damage.                                                                                       |
| **dmg** | Damage Per Round (DPR) or Total Damage Dealt. The total health points of damage the player has inflicted on enemies.                                                                                    |
| **hsp** | Headshot Percentage: (Headshot Kills / Total Kills) \* 100. The percentage of the player's kills that were headshots.                                                                                    |
| **kdr** | Kill/Death Ratio: kills divided by deaths. If the player has zero deaths, this is represented as the total number of kills.                                                                            |
| **adr** | Average Damage per Round: total damage dealt divided by the number of rounds the player has played.                                                                                                    |
| **mvp** | The number of times the player has been the Most Valuable Player of a round, based on impactful actions.                                                                                               |
| **ef** | Enemies Flashed: The number of opponents successfully blinded by the player's flashbang grenades.                                                                                                      |
| **ud** | Utility Damage: The total damage dealt to enemies using utility grenades (HE Grenades, Molotovs, Incendiary Grenades).                                                                                   |
| **3k** | The number of rounds in which the player achieved exactly 3 kills.                                                                                                                                     |
| **4k** | The number of rounds in which the player achieved exactly 4 kills.                                                                                                                                     |
| **5k** | The number of rounds in which the player achieved 5 kills (an "ace").                                                                                                                                  |
| **clutchk** | Clutch Kills: The number of kills the player secured while being the last player alive on their team against one or more opponents. This often counts total kills within successful clutch rounds won. |
| **firstk** | First Kills / Opening Kills: The number of rounds in which the player secured the first kill.                                                                                                         |
| **pistolk** | Pistol Kills: The total number of kills achieved using pistols.                                                                                                                                         |
| **sniperk** | Sniper Kills: The total number of kills achieved using sniper rifles (e.g., AWP, SSG 08, SCAR-20, G3SG1).                                                                                                |
| **blindk** | Blind Kills: The number of kills achieved while the player's vision was significantly impaired by a flashbang.                                                                                           |
| **bombk** | Kills directly resulting from a planted bomb explosion.                                                                                                                                               |
| **firedmg** | Fire Damage: The total damage dealt to enemies specifically by Molotov cocktails or Incendiary Grenades.                                                                                                |
| **uniquek** | The number of unique opponents the player has killed.                                                                                                                                                  |
| **dinks** | The number of times the player hit an enemy in the head without killing them.                                                                                                                          |
| **chickenk** | The total number of chickens killed by the player.                                                                                                                                                     |

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

## General Log Syntax:
