<!-- TOC depth:6 withLinks:1 updateOnSave:1 -->
- [CS:GO deployment](#csgo-deployment)
- [CS:GO SRCDS launch](#csgo-srcds-launch)
	- [SRCDS options](#srcds-options)
	- [game_mode and game_type](#gamemode-and-gametype)
	- [server.cfg](#servercfg)
	- [bot_](#bot)
	- [mp_](#mp)
	- [sv_](#sv)
	- [others](#others)
- [CS:GO commands](#csgo-commands)
<!-- /TOC -->
****************************************

# CS:GO deployment
    apt-get install -y lib32gcc1
    useradd -m steam -s /bin/bash
    su - steam && cd
    wget http://media.steampowered.com/installer/steamcmd_linux.tar.gz
    tar xzf steamcmd_linux.tar.gz
    ./steamcmd.sh +login anonymous +force_install_dir ~/csgo_srv +app_update 740 +quit
		mkdir -p ~/.steam/sdk32 && cp linux32/* ~/.steam/sdk32

# CS:GO SRCDS launch
    ./srcds_run -game csgo -console -usercon +game_type 0 +game_mode 1 +mapgroup mg_active +map de_dust2
## SRCDS options
		-autoupdate The server is searching for updates on startup (Linux/Unix only)
    -ip Specifies the address to use for the bind(2) syscall.
    -maxplayers <number>
		-nobots
		-nohltv
    -port <variable> - Sets port for a listen server. Default is 27015
    -tickrate <number>
		-timeout <number> Sleep for <number> seconds before restarting a failed server.
    -threads -
    -usercon Enables RCON for CS:GO Servers
## SRCDS console vars
    +hostname "Hostname" - Specifies the name of the server (Spaces between words won't work here!).
    +ip - Specifies the IP when multiple network cards are installed (Old, but still works).
    +map <map> - Specifies which map to start.
    +maxplayers <number> - Specifies how many player slots the server can contain (Old, but still works).
    +hostport <port> - Specifies the host port (Port specification before -port was added, but can still be used).
    +clientport <port> - Specifies the client port (The port the server advertises to clients, the same as -port and clientport in console)
    +sv_lan <0/1> - If set to 1, server is only available in Local Area Network (LAN).
[See More](https://developer.valvesoftware.com/wiki/Command_Line_Options#Source_Dedicated_Server)
## game_mode and game_type
| Mode                | game_mode | game_type |
|---------------------|-----------|-----------|
| Classic Casual      |     0     |     0     |
| Classic Competitive |     0     |     1     |
| Arms Race           |     1     |     0     |
| Demolition          |     1     |     1     |
## server.cfg
    hostname "Counter-Strike: Global Offensive Dedicated Server"
    rcon_password "yourrconpassword"
    sv_password "" //Only set this if you intend to have a private server!
## bot_
    bot_difficulty 1
    bot_chatter "off"
    bot_join_after_player 1
    bot_quota 10
    bot_quota_mode "fill"
## mp_
    mp_autoteambalance 0
    mp_buytime 15           // How many seconds after round start players can buy items for
    mp_c4timer 35           // How long from when the C4 is armed until it blows
    mp_death_drop_defuser 1 // Drop defuser on player death
    mp_death_drop_grenade 2 // Which grenade to drop on player death: 0=none, 1=best, 2=current or best
    mp_death_drop_gun 1     // Which gun to drop on player death: 0=none, 1=best, 2=current or best
    mp_do_warmup_period 1   // Whether or not to do a warmup period at the start of a match
    mp_forcecamera 1        // Restricts spectator modes for dead players
    mp_free_armor 0         // Determines whether armor and helmet are given automatically
    mp_freezetime 12        // How many seconds to keep players frozen when the round starts
    mp_friendlyfire 1       // Allows team members to injure other members of their team
    mp_halftime 1           // Determines whether or not the match has a team-swapping halftime event
    mp_halftime_duration 15 // Number of seconds that halftime lasts
    mp_maxrounds 30         // max number of rounds to play before server changes maps
    mp_join_grace_time 15   // The amount of time players can join teams after a round has started
    mp_match_end_restart 0  // Defines whether a map should be restarted after a game has ended
    mp_roundtime            // How many minutes each round takes.
    mp_roundtime_defuse     // How many minutes each round of Bomb Defuse takes. If 0 then use mp_roundtime instead.
    mp_roundtime_hostage    // How many minutes each round of Hostage Rescue takes. If 0 then use mp_roundtime instead.
    mp_solid_teammates 1 		// Determines whether teammates are solid or not
    mp_startmoney 800       // amount of money each player gets when they reset
		mp_teamcashawards 1			// Teams can earn money by performing in-game actions
		mp_timelimit 0					// game time per map in minutes
    mp_tkpunish 0           // Will a TK'er be punished in the next round?  {0=no,  1=yes}
## sv_
    sv_allow_votes 0        // Allow voting?
    sv_alltalk 0            // Players can hear all other players' voice communication, no team restrictions
    sv_consistency 0        // Whether the server enforces file consistency for critical files
    sv_contact 0						// Contact email for server sysop
    sv_cheats 0             //This should always be set, so you know it's not on
    sv_deadtalk 0						// Dead players can speak (voice, text) to the living
    sv_forcepreload 0				// Force server side preloading.
    sv_full_alltalk 0				// Any player (including Spectator team) can speak to any other player
    sv_kick_players_with_cooldown 0 // (0: do not kick; 1: kick Untrusted players; 2: kick players with any cooldown)
    sv_kick_ban_duration 0  // How long should a kick ban from the server should last (in minutes)
    sv_lan 0 								//This should always be set, so you know it's not on
    sv_log_onefile 0				// Log server information to only one file
    sv_logbans 1						// Log server bans in the server logs
    sv_logecho 1						// Echo log information to the console
    sv_logfile 1						// Log server information in the log file
    sv_logsdir logfiles     // Folder in the game directory where server logs will be stored
    sv_maxrate 0						// min. 0.000000 max. 30000.000000 replicated  Max bandwidth rate allowed on server, 0 == unlimited
    sv_mincmdrate 30				// This sets the minimum value for cl_cmdrate. 0 == unlimited
    sv_minrate 20000				// Min bandwidth rate allowed on server, 0 == unlimited
    sv_minupdaterate        // Minimum updates per second that the server will allow
    sv_pure 1
    sv_pure_kick_clients 1  // If set to 1, the server will kick clients with mismatching files. Otherwise, it will issue a warning to the client
    sv_region 3             // The region of the world to report this server in
		sv_tags "tag1,tag2,tag3"
    sv_voiceenable 1
## others
    writeid                 // Writes a list of permanently-banned user IDs to banned_user.cfg.
    writeip                 // Save the ban list to banned_ip.cfg

# CS:GO commands
    banid                   // Add a user ID to the ban list.
    banip                   // Add an IP address to the ban list.
    bot_add bot_add <t|ct> <type> <difficulty> <name> // Adds a bot matching the given criteria.
    bot_add_ct bot_add_ct <type> <difficulty> <name>  // Adds a Counter-Terrorist bot matching the given criteria.
    bot_add_t bot_add_t <type> <difficulty> <name>    // Adds a terrorist bot matching the given criteria.
    bot_all_weapons         // Allows the bots to use all weapons
    bot_kick bot_kick <all> <t|ct> <type> <difficulty> <name> // Kicks a specific bot, or all bots, matching the given criteria.
    bot_kill bot_kill <all> <t|ct> <type> <difficulty> <name> // Kills a specific bot, or all bots, matching the given criteria.
    bot_knives_only         // Restricts the bots to only using knives
    changelevel Change server to the specified map
    kick                    // Kick a player by name.
    kickid                  // Kick a player by userid or uniqueid, with a message.
    kickid_ex               // Kick a player by userid or uniqueid, provide a force-the-kick flag and also assign a message.
    listid                  // Lists banned users.
    listip                  // List IP addresses on the ban list.
    mapgroup                // Specify a map group
    maps                    // Displays list of maps.
    maxplayers              // Change the maximum number of players allowed on this server.
    mp_scrambleteams        // Scramble the teams and restart the game
    stats                   // Prints server performance variables
    status                  // Display map and connection status.
    user                    // Show user data.
    users                   // Show user info for players on server.
    version                 // Print version info string.
    voice_mute              // Mute a specific Steam user
