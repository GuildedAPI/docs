# Users

Users are everywhere. They're in our servers, our DMs, our friends lists.. and in this documentation page!

### User Object

###### User Structure

| Field               | Type                                         | Description                                                  |
|---------------------|----------------------------------------------|--------------------------------------------------------------|
| id                  | [generic id](/reference#generic-object-ids)  | the user's id                                                |
| name                | string                                       | the user's username, not unique across the platform          |
| subdomain           | ?string                                      | the user's unique profile url                                |
| aliases             | array                                        | the linked games on the user's profile                       |
| email               | ?string                                      | the user's email. null if this is not you                    |
| serviceEmail        | ?string                                      | ?                                                            |
| profilePicture      | string (url)                                 | the user's avatar url                                        |
| profilePictureSm    | string (url)                                 | ^                                                            |
| profilePictureLg    | string (url)                                 | ^                                                            |
| profilePictureBlur  | string (url)                                 | ^                                                            |
| profileBannerSm     | ?string (url)                                | the user's banner url                                        |
| profileBannerLg     | ?string (url)                                | ^                                                            |
| profileBannerBlur   | ?string (url)                                | ^                                                            |
| joinDate            | ISO8601 timestamp                            | when this user's account was created                         |
| steamId             | ?string                                      | this user's steam id, if linked                              |
| userStatus          | [user status object](#user-status-object)    | this user's current activity/"status"                        |
| userPresenceStatus  | integer                                      | this [user's presence](#user-presence) (online, idle, etc)   |
| userTransientStatus | [transient status object](#transient-status) | this user's transient status (game, streaming, ?)            |
| moderationStatus    | ?                                            | ?                                                            |
| aboutInfo           | [about info object](#about-info-object)      | this user's bio and tagline                                  |
| lastOnline          | ISO8601 timestamp                            | when this user was last online                               |
| stonks?             | integer                                      | number of "stonks" the user has gotten from inviting people  |
| flairInfos?         | array of [flair objects](#flair-object)      | the flairs the user has on their profile                     |

###### Example User

```json
{
  "id": "EdVMVKR4",
  "name": "shay",
  "subdomain": "shayy",
  "aliases": [
    {
      "alias": "shay",
      "discriminator": null,
      "name": "shay",
      "createdAt": "2021-03-03T21:02:19.901444+00:00",
      "userId": "EdVMVKR4",
      "gameId": 220015,
      "socialLinkSource": null,
      "additionalInfo": {},
      "editedAt": "2021-03-03T21:02:19.901444+00:00",
      "socialLinkHandle": null,
      "playerInfo": null
    }
  ],
  "email": null,
  "profilePictureSm": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/UserAvatar/74bfc8be9425a926a1f48d9b078509bc-Large.png?w=450&h=450",
  "profilePicture": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/UserAvatar/74bfc8be9425a926a1f48d9b078509bc-Large.png?w=450&h=450",
  "profilePictureLg": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/UserAvatar/74bfc8be9425a926a1f48d9b078509bc-Large.png?w=450&h=450",
  "profilePictureBlur": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/UserAvatar/74bfc8be9425a926a1f48d9b078509bc-Large.png?w=450&h=450",
  "profileBannerBlur": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/UserBanner/acaa9d0f78dd8cdd93f3ce44d14c0260-Hero.png?w=1500&h=500",
  "profileBannerLg": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/UserBanner/acaa9d0f78dd8cdd93f3ce44d14c0260-Hero.png?w=1500&h=500",
  "profileBannerSm": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/UserBanner/acaa9d0f78dd8cdd93f3ce44d14c0260-Hero.png?w=1500&h=500",
  "joinDate": "2020-07-27T14:07:28.336Z",
  "steamId": null,
  "userStatus": {
    "content": null,
    "customReactionId": 294765,
    "customReaction": {
      "id": 294765,
      "name": "ablobwobwork",
      "png": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/CustomReaction/d7cf013a4d01460a81186e65f1f8c12a-Full.webp?w=120&h=120&ia=1",
      "webp": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/CustomReaction/d7cf013a4d01460a81186e65f1f8c12a-Full.webp?w=120&h=120&ia=1",
      "apng": null
    }
  },
  "moderationStatus": null,
  "aboutInfo": {
    "bio": "i own the guilded API server, go join for bot discussion, libraries, and general programming whatnot guilded.gg/guilded-api",
    "tagLine": "computer user"
  },
  "lastOnline": "2021-03-08T00:24:49.144Z",
  "serviceEmail": null,
  "userPresenceStatus": 1,
  "userTransientStatus": null,
  "stonks": 2
}
```

### User Presence

| Value | Meaning           |
|-------|-------------------|
| 1     | online            |
| 2     | idle              |
| 3     | dnd               |
| 4     | offline/invisible |

### Transient Status Object

| Field           | Type                                | Description                              |
|-----------------|-------------------------------------|------------------------------------------|
| id              | integer                             |                                          |
| gameId          | ?integer                            | the game's id this status is for (see the game ids table below for known valid values) |
| name?           | string                              | the game's name (should be included if gameId is null)                                 |
| type            | string                              | the type of status ("gamepresence", ?)   |
| startedAt       | ISO8601 timestamp                   | when this status started                 |
| guildedClientId | [uuid](/reference#snowflakes-uuids) | the client's id that this status is from |

###### Game IDs

If you want to use this list in your application, feel free to grab it in [JSON format from our datatables repo](https://git.guildedapi.com/datatables/blob/main/games.json).

| ID     | Game                                     |
|--------|------------------------------------------|
| 10100  | Overwatch                                |
| 10200  | League of Legends                        |
| 10300  | CounterStrike: Global Offensive          |
| 10400  | Dota 2                                   |
| 10500  | Minecraft                                |
| 10600  | World of Tanks                           |
| 10700  | Battlefield 4                            |
| 10800  | World of Warcraft                        |
| 10900  | Call of Duty: Black Ops III              |
| 101000 | Smite                                    |
| 101100 | Heroes of the Storm                      |
| 101200 | Halo 5: Guardians                        |
| 101300 | Tom Clancy’s The Division                |
| 101400 | Guild Wars 2                             |
| 101500 | Final Fantasy XIV: Shadowbringers        |
| 101600 | Starcraft II                             |
| 101700 | The Elder Scrolls Online                 |
| 101800 | Team Fortress II                         |
| 101900 | Destiny                                  |
| 102000 | Terraria                                 |
| 102100 | Star Wars: The Old Republic              |
| 102200 | ArcheAge Unchained                       |
| 102300 | Runescape                                |
| 102400 | Diablo III                               |
| 102500 | Vainglory                                |
| 102600 | Clash of Clans                           |
| 102700 | Rocket League                            |
| 102800 | Pokémon GO                               |
| 102900 | Grand Theft Auto V                       |
| 103000 | EVE Online                               |
| 103100 | ARK: Survival Evolved                    |
| 103200 | Arma 3                                   |
| 103300 | Battlefield 1                            |
| 103400 | Black Desert Online                      |
| 103500 | Call of Duty: Infinite Warfare           |
| 103600 | DayZ                                     |
| 103700 | Elite Dangerous                          |
| 103800 | H1Z1: Just Survive                       |
| 103900 | Miscreated                               |
| 104000 | Neverwinter                              |
| 104100 | Path of Exile                            |
| 104200 | Rainbow Six Siege                        |
| 104300 | Rust                                     |
| 104400 | Starbound                                |
| 104500 | Star Citizen                             |
| 104600 | Tabletop Simulator                       |
| 104700 | Secret World Legends                     |
| 104800 | Titanfall                                |
| 104900 | Titanfall 2                              |
| 105000 | Total War: WARHAMMER                     |
| 105100 | Warframe                                 |
| 105200 | No Man's Sky                             |
| 105300 | Dragon Quest X                           |
| 105400 | Ingress                                  |
| 105500 | Phantasy Star Online 2                   |
| 105600 | Skyforge                                 |
| 105700 | Tera                                     |
| 105800 | Wildstar                                 |
| 105900 | Metal Gear Online                        |
| 106000 | Super Smash Bros. for Wii U              |
| 106100 | 7 Days to Die                            |
| 106200 | Air Mech                                 |
| 106300 | Battleborn                               |
| 106400 | Blacklight: Retribution                  |
| 106500 | Battlecry                                |
| 106600 | Chivalry                                 |
| 106700 | Civilization VI                          |
| 106800 | Dragon Ball Xenoverse 2                  |
| 106900 | Dragon Nest                              |
| 107000 | The Elder Scrolls: Legends               |
| 107100 | EVE: Valkyrie                            |
| 107200 | For Honor                                |
| 107300 | Forza Horizon 3                          |
| 107400 | Gears of War 4                           |
| 107500 | Gigantic                                 |
| 107600 | Gran Turismo Sport                       |
| 107700 | Halo Wars 2                              |
| 107800 | Hawken                                   |
| 107900 | Hearthstone                              |
| 108000 | Heroes of Newerth                        |
| 108100 | LawBreakers                              |
| 108200 | Marvel Heroes                            |
| 108300 | Paragon                                  |
| 108400 | Portal Knights                           |
| 108500 | Riders of Icarus                         |
| 108600 | Scrolls                                  |
| 108700 | Stardew Valley                           |
| 108800 | Star Wars Battlefront                    |
| 108900 | Street Fighter V                         |
| 109000 | Strife                                   |
| 109100 | Tekken 7                                 |
| 109200 | The Crew                                 |
| 109300 | Trove                                    |
| 109400 | Warhammer 40000: Eternal Crusade         |
| 109500 | XCOM2                                    |
| 109600 | Halo Wars                                |
| 109700 | APB Reloaded                             |
| 109800 | Arena of Fate                            |
| 109900 | Call of Duty: Advanced Warfare           |
| 110000 | Crossout                                 |
| 110100 | Crusader Kings 2                         |
| 110200 | Duelyst                                  |
| 110300 | Dungeon Fighter Online                   |
| 110400 | Evolve                                   |
| 110500 | Ghost Recon: Wildlands                   |
| 110600 | Guns of Icarus                           |
| 110700 | Heroes & Generals                        |
| 110800 | Killing Floor 2                          |
| 110900 | King of Fighters XIV                     |
| 111000 | Master of Orion                          |
| 111100 | Battlefield Hardline                     |
| 111200 | Of Kings And Men                         |
| 111300 | Planetside 2                             |
| 111400 | Robocraft                                |
| 111500 | Savage Resurrection                      |
| 111600 | Shop Heroes                              |
| 111700 | Warface                                  |
| 111800 | War Thunder                              |
| 111900 | The Walking Dead: Road to Survival       |
| 112100 | Aion                                     |
| 112200 | Armello                                  |
| 112300 | Atlas Reactor                            |
| 112400 | Blade & Soul                             |
| 112500 | Camelot Unchained                        |
| 112600 | Lineage 2                                |
| 112700 | Master X Master                          |
| 112800 | Onward                                   |
| 112900 | RIGS Mechanized Combat League            |
| 113000 | The Black Watchmen                       |
| 113100 | Critical Ops                             |
| 113200 | Dead Or Alive 5                          |
| 113300 | Depth                                    |
| 113400 | DirtyBomb                                |
| 113500 | Plants vs Zombies 2                      |
| 113600 | Pro Evolution Soccer                     |
| 113700 | Star Trek Online                         |
| 113800 | Strike Vector                            |
| 113900 | The Tomorrow Children                    |
| 114000 | Transformers: Fall of Cybertron          |
| 115000 | Ashes of the Singularity                 |
| 116000 | Battlerite                               |
| 117000 | Borderlands                              |
| 118000 | Borderlands: The Pre-Sequel              |
| 119000 | Civilization: Beyond Earth               |
| 120000 | Creativerse                              |
| 121000 | Day of Infamy                            |
| 122000 | Dead by Daylight                         |
| 123000 | Divinity: Original Sin                   |
| 124000 | Divinity: Original Sin 2                 |
| 125000 | Endless Space 2                          |
| 126000 | E.T. Armies                              |
| 127000 | EverQuest 2                              |
| 128000 | Faeria                                   |
| 129000 | FIFA 17                                  |
| 130000 | Formula One 2016                         |
| 131000 | Fractured Space                          |
| 132000 | Garry's Mod                              |
| 133000 | Helldivers                               |
| 134000 | Homeworld                                |
| 135000 | Hybrid Wars                              |
| 136000 | Insurgency                               |
| 137000 | Left 4 Dead 2                            |
| 139000 | Loadout                                  |
| 140000 | Looterkings                              |
| 141000 | Madden NFL 17                            |
| 142000 | MechWarrior Online                       |
| 143000 | Mercenary Kings                          |
| 144000 | Metal Gear Survive                       |
| 145000 | Mirage: Arcane Warfare                   |
| 146000 | Morphies Law                             |
| 147000 | NBA 2K17                                 |
| 148000 | Osiris: New Dawn                         |
| 149000 | Out of Ammo                              |
| 150000 | Paladins                                 |
| 151000 | Pit People                               |
| 152000 | Pokken Tournament                        |
| 153000 | Quake Champions                          |
| 154000 | Sea of Thieves                           |
| 155000 | Shard Games                              |
| 156000 | ShootMania Storm                         |
| 157000 | SMASH+GRAB                               |
| 158000 | Splatoon                                 |
| 159000 | Summoners War                            |
| 160000 | TowerFall                                |
| 161000 | Toxikk                                   |
| 162000 | Tribes: Ascend                           |
| 163000 | Umbrella Corps                           |
| 164000 | Uncharted 4                              |
| 165000 | Unreal Tournament                        |
| 166000 | Versus: Battle of the Gladiator          |
| 168000 | Armored Warfare                          |
| 169000 | Breakaway                                |
| 170000 | Clash Royale                             |
| 171000 | H1Z1: King of the Kill                   |
| 172000 | Infinite Tanks                           |
| 173000 | Midair                                   |
| 174000 | Nidhogg                                  |
| 175000 | Nidhogg 2                                |
| 176000 | Redout                                   |
| 177000 | Shards Online                            |
| 178000 | Space Hulk: Deathwing                    |
| 179000 | Watch Dogs 2                             |
| 180000 | World of Warplanes                       |
| 181000 | World of Warships                        |
| 182000 | Astroneer                                |
| 183000 | Endless Legend                           |
| 184000 | Order & Chaos Online                     |
| 185000 | Project Genom                            |
| 186000 | Squad                                    |
| 187000 | Order & Chaos 2                          |
| 188000 | Conan Exiles                             |
| 189000 | MU Legend                                |
| 190000 | Mutant Football League                   |
| 191000 | Revelation Online                        |
| 192000 | Snow                                     |
| 193000 | Absolver                                 |
| 194000 | Agents of Mayhem                         |
| 195000 | A Hat in Time                            |
| 196000 | Battalion 1944                           |
| 197000 | Battletech                               |
| 198000 | Crackdown 3                              |
| 199000 | Dauntless                                |
| 200000 | EverQuest                                |
| 201000 | Fortnite Battle Royale                   |
| 202000 | Friday the 13th: The Game                |
| 203000 | Mass Effect: Andromeda                   |
| 204000 | Lord of the Rings Online                 |
| 205000 | Pantheon                                 |
| 206000 | Pillars of Eternity 2                    |
| 207000 | Red Dead Redemption 2                    |
| 208000 | Rift                                     |
| 209000 | State of Decay 2                         |
| 210000 | Albion Online                            |
| 211000 | Chronicles of Elyria                     |
| 212000 | Dark and Light                           |
| 213000 | Grim Dawn                                |
| 214000 | Playerunknown's Battlegrounds            |
| 215000 | Blockstorm                               |
| 216000 | Destiny 2                                |
| 217000 | MapleStory                               |
| 218000 | Gloria Victis                            |
| 219000 | Starcraft                                |
| 220000 | Tribal Wars 2                            |
| 220001 | Asta Online                              |
| 220002 | Awesomenauts                             |
| 220003 | Bless                                    |
| 220004 | Devilian                                 |
| 220005 | Gwent                                    |
| 220006 | STEAM HAMMER                             |
| 220007 | The Unspoken                             |
| 220008 | Titan Siege                              |
| 220009 | Call of Duty: WWII                       |
| 220010 | Age of Empires II                        |
| 220011 | Crossfire                                |
| 220012 | Foxhole                                  |
| 220013 | Hearts of Iron 4                         |
| 220014 | Kingdom Hearts Union χ                   |
| 220015 | Mario Kart 8                             |
| 220016 | Marvel vs Capcom: Infinite               |
| 220017 | Rising Storm 2: Vietnam                  |
| 220018 | Star Wars Galaxy of Heroes               |
| 220019 | Stellaris                                |
| 220020 | Surviving Mars                           |
| 220022 | Wakfu                                    |
| 220023 | Worlds Adrift                            |
| 220024 | Arena of Valor                           |
| 220025 | Aura Kingdom                             |
| 220026 | Black Squad                              |
| 220027 | Castle Story                             |
| 220028 | Citadel                                  |
| 220029 | Crowfall                                 |
| 220030 | Crusaders of Light                       |
| 220031 | Divekick                                 |
| 220032 | Escape from Tarkov                       |
| 220033 | Everybody's Golf                         |
| 220034 | FutureFight                              |
| 220035 | Hyper Universe                           |
| 220036 | Keystone                                 |
| 220037 | Kritika Online                           |
| 220038 | Livelock                                 |
| 220039 | MOBA Legends                             |
| 220040 | NextDay                                  |
| 220041 | Payday 2                                 |
| 220042 | Planet Coaster                           |
| 220043 | PWND                                     |
| 220044 | Roblox                                   |
| 220045 | Shadowverse                              |
| 220046 | Spiral Knights                           |
| 220047 | Splatoon 2                               |
| 220048 | Tower Unite                              |
| 220049 | Tree of Life                             |
| 220050 | Witch It                                 |
| 220051 | Alicia Online                            |
| 220052 | Elyon - Ascent: Infinite Realm           |
| 220053 | Ashes of Creation                        |
| 220054 | Brawl Stars                              |
| 220055 | Darkfall: New Dawn                       |
| 220056 | Darwin Project                           |
| 220057 | Dissidia: Final Fantasy NT               |
| 220058 | Dofus                                    |
| 220059 | Dragon Ball FighterZ                     |
| 220060 | Dual Universe                            |
| 220061 | Dungeons & Dragons Online                |
| 220062 | Elsword                                  |
| 220063 | Heliborne                                |
| 220064 | Lineage 2: Revolution                    |
| 220065 | Lost Ark                                 |
| 220066 | Orcs Must Die! Unchained                 |
| 220067 | Raiders of the Broken Planet             |
| 220068 | Star Wars Battlefront II                 |
| 220069 | Tantra Rumble                            |
| 220070 | The Exiled                               |
| 220071 | Total War: ARENA                         |
| 220072 | Total War: WARHAMMER II                  |
| 220073 | Unison League                            |
| 220074 | ValnirRok                                |
| 221074 | Brawlhalla                               |
| 222074 | Age of Conan: Unchained                  |
| 223074 | Clash of Kings                           |
| 224074 | Closers                                  |
| 225074 | Dungeons & Dragons                       |
| 226074 | Eternal                                  |
| 227074 | Fantasy Strike                           |
| 228074 | Fractured                                |
| 229074 | Free Fire                                |
| 230074 | Harry Potter: Wizards Unite              |
| 231074 | Hell Let Loose                           |
| 232074 | Holdfast: Nations At War                 |
| 233074 | Honkai Impact 3rd                        |
| 234074 | iRacing                                  |
| 235074 | Ironsight                                |
| 236074 | Kerbal Space Program                     |
| 237074 | King of Avalon                           |
| 238074 | Laser League                             |
| 239074 | Maelstrom                                |
| 240074 | Magic: The Gathering                     |
| 241074 | Monster Hunter: World                    |
| 242074 | OrbusVR                                  |
| 243074 | Pathfinder                               |
| 244074 | Post Scriptum                            |
| 245074 | Shadowrun                                |
| 246074 | SoulWorker                               |
| 247074 | Space Junkies                            |
| 248074 | SpellForce 3                             |
| 249074 | Squids From Space                        |
| 250074 | Stick Fight: The Game                    |
| 251074 | 13th Age                                 |
| 252074 | The Wild Eight                           |
| 253074 | Tree of Savior                           |
| 254074 | Valiant Force                            |
| 255074 | VRChat                                   |
| 256074 | War Dragons                              |
| 257074 | Ylands                                   |
| 258074 | MapleStory 2                             |
| 259074 | Call of Duty: Black Ops 4                |
| 260074 | Sky Noon                                 |
| 261074 | Unturned                                 |
| 262074 | War Robots                               |
| 263074 | Game of Thrones: Conquest                |
| 264074 | Marvel Strike Force                      |
| 265074 | Pokémon TCG Online                       |
| 266074 | SCP: Secret Laboratory                   |
| 267074 | Star Wars Commander                      |
| 268074 | Raft                                     |
| 269074 | Tibia                                    |
| 270074 | TerraTech                                |
| 271074 | Project: Gorgon                          |
| 272074 | Life Is Feudal: MMO                      |
| 273074 | Battlefield V                            |
| 274074 | World of Tanks Blitz                     |
| 275074 | Fallout: 76                              |
| 276074 | Infinite Flight                          |
| 277074 | Europa Universalis IV                    |
| 278074 | Rage 2                                   |
| 279074 | Realm Royale                             |
| 280074 | Skull & Bones                            |
| 281074 | Super Smash Bros. Ultimate               |
| 282074 | Gears 5                                  |
| 283074 | The Elder Scrolls: Blades                |
| 284074 | Generation Zero                          |
| 285074 | Rend                                     |
| 286074 | The Division 2                           |
| 287074 | Defiance 2050                            |
| 288074 | Mavericks                                |
| 289074 | Strange Brigade                          |
| 290074 | Mario Tennis Aces                        |
| 291074 | Eco                                      |
| 292074 | Town of Salem                            |
| 293074 | Legends of Aria                          |
| 294074 | Ragnarok Online                          |
| 295074 | The Crew 2                               |
| 296074 | Captain Tsubasa: Dream Team              |
| 297074 | Remnant: From the Ashes                  |
| 298074 | FIFA 19                                  |
| 299074 | Madden NFL 19                            |
| 300074 | Fractured Lands                          |
| 301074 | Planet of Heroes                         |
| 302074 | Satisfactory                             |
| 303074 | Roof Rage                                |
| 304074 | Lords Mobile                             |
| 305074 | TrackMania                               |
| 306074 | Heroes Evolved                           |
| 307074 | PixARK                                   |
| 308074 | Avorion                                  |
| 309074 | PUBG Mobile                              |
| 310074 | Football Manager                         |
| 311074 | Adventure Quest 3D                       |
| 312074 | Anthem                                   |
| 313074 | Celtic Heroes                            |
| 314074 | Dungeon Hunter Champions                 |
| 315074 | Modern Combat Versus                     |
| 316074 | Space Engineers                          |
| 317074 | SCUM                                     |
| 318074 | Artifact                                 |
| 319074 | Don't Starve Together                    |
| 320074 | Warhammer: Vermintide II                 |
| 321074 | Interstellar Rift                        |
| 322074 | Battlerite Royale                        |
| 323074 | osu"                                     |
| 324074 | Rise of Kingdoms                         |
| 325074 | New World                                |
| 326074 | GunBound                                 |
| 327074 | Mount & Blade Warband                    |
| 328074 | SoulCalibur VI                           |
| 329074 | World War 3                              |
| 332074 | America's Army: Proving Grounds          |
| 333074 | Project CARS 2                           |
| 334074 | Forza Horizon 4                          |
| 335074 | Warcraft III: Reforged                   |
| 336074 | Rise of Agon                             |
| 337074 | Ring of Elysium                          |
| 338074 | Temtem                                   |
| 339074 | FIFA Mobile                              |
| 340074 | Atlas                                    |
| 341074 | Digital Combat Simulator                 |
| 342074 | Last Year: The Nightmare                 |
| 343074 | The Forest                               |
| 344074 | Eden Rising                              |
| 345074 | Creative Destruction                     |
| 346074 | Final Fantasy XI                         |
| 347074 | Insurgency: Sandstorm                    |
| 348074 | Hurtworld                                |
| 349074 | Mortal Kombat 11                         |
| 350074 | World of Warcraft Classic                |
| 351074 | Pantropy                                 |
| 352074 | Apex Legends                             |
| 353074 | Rules of Survival                        |
| 354074 | Allods Online                            |
| 355074 | Star Trek Fleet Command                  |
| 356074 | Empyrion: Galactic Survival              |
| 357074 | Mordhau                                  |
| 358074 | Echo Arena                               |
| 359074 | Echo Combat                              |
| 360074 | Planetside Arena                         |
| 361074 | Halo                                     |
| 362074 | Halo 2                                   |
| 363074 | Halo 3                                   |
| 364074 | Halo: Reach                              |
| 365074 | Halo 4                                   |
| 366074 | Risk of Rain 2                           |
| 367074 | Tiny Tanks                               |
| 368074 | Mobile Legends: Bang Bang                |
| 369074 | LifeAfter                                |
| 370074 | Splitgate: Arena Warfare                 |
| 371074 | Total War: Three Kingdoms                |
| 372074 | Astellia Online                          |
| 373074 | FRAG Pro Shooter                         |
| 374074 | Rocket Arena                             |
| 375074 | Auto Chess                               |
| 376074 | Bleeding Edge                            |
| 377074 | Dota Underlords                          |
| 378074 | Halo Infinite                            |
| 379074 | Last Oasis                               |
| 380074 | Midnight Ghost Hunt                      |
| 381074 | Minecraft Dungeons                       |
| 382074 | Outriders                                |
| 383074 | Roller Champions                         |
| 384074 | Teamfight Tactics                        |
| 385074 | Battletoads                              |
| 386074 | Clans of Reign                           |
| 387074 | Hytale                                   |
| 388074 | Killsquad                                |
| 389074 | Kingdom Under Fire 2                     |
| 390074 | Krunker                                  |
| 391074 | Call of Duty: Modern Warfare             |
| 392074 | Pokemon Sword & Shield                   |
| 393074 | Skyweaver                                |
| 394074 | Streets of Rage 4                        |
| 395074 | SYNCED: Off-Planet                       |
| 396074 | The Cycle                                |
| 397074 | Call of Duty: Mobile                     |
| 398074 | FIFA 20                                  |
| 399074 | Borderlands 3                            |
| 400074 | Warhammer 40K: Inquisitor - Martyr       |
| 401074 | Black Desert Mobile                      |
| 402074 | GTFO                                     |
| 403074 | Hunt: Showdown                           |
| 404074 | Old School RuneScape                     |
| 405074 | Warfield                                 |
| 406074 | Magic the Gathering: Arena               |
| 407074 | Legends of Runeterra                     |
| 408074 | Wolcen                                   |
| 409074 | American Truck Simulator                 |
| 410074 | Boundless                                |
| 411074 | Broomstick League                        |
| 412074 | Call of Duty: Warzone                    |
| 413074 | Disintegration                           |
| 414074 | Euro Truck Simulator 2                   |
| 415074 | Kerbal Space Program 2                   |
| 416074 | Quantum League                           |
| 417074 | Spellbreak                               |
| 418074 | State of Survival                        |
| 419074 | Rogue Company                            |
| 420074 | Black Mesa                               |
| 422074 | Valorant                                 |
| 423074 | Starborne                                |
| 424074 | Torchlight III                           |
| 425074 | Chivalry II                              |
| 426074 | Predator: Hunting Grounds                |
| 427074 | Crucible                                 |
| 428074 | KartRider: Drift                         |
| 429074 | Ninjala                                  |
| 430074 | Assetto Corsa                            |
| 431074 | Assetto Corsa Competizione               |
| 432074 | Boom Beach                               |
| 433074 | Conqueror's Blade                        |
| 434074 | Crossfire X                              |
| 435074 | DeadSide                                 |
| 436074 | Dragalia Lost                            |
| 437074 | Killer Instinct                          |
| 438074 | Pokémon Unite                            |
| 439074 | Star Wars: Squadrons                     |
| 440074 | TrackMania                               |
| 441074 | World of Warships: Legends               |
| 442074 | Seven Deadly Sins: Grand Cross           |
| 443074 | ARMS                                     |
| 444074 | Blightbound                              |
| 445074 | Contractors                              |
| 446074 | Crash Team Racing: Nitro-Fueled          |
| 447074 | Tom Clancy's Elite Squad                 |
| 448074 | Fragsurf                                 |
| 449074 | Grounded                                 |
| 450074 | Marvel's Avengers                        |
| 451074 | Hyper Scape                              |
| 452074 | NBA 2K21                                 |
| 453074 | Need For Speed Heat                      |
| 454074 | Plan 8                                   |
| 455074 | Teppen                                   |
| 456074 | Fall Guys                                |
| 457074 | Million Lords                            |
| 458074 | Tetris 99                                |
| 459074 | Among Us                                 |
| 460074 | Diabotical                               |
| 461074 | League of Legends Wild Rift              |
| 462074 | Genshin Impact                           |
| 463074 | EVE Echoes                               |
| 464074 | Baldur's Gate III                        |
| 465074 | Phasmophobia                             |
| 466074 | Call of Duty: Black Ops Cold War         |
| 467074 | SWORD ART ONLINE Alicization Lycoris     |
| 468074 | NARUTO TO BORUTO: Shinobi Striker        |
| 469074 | NARUTO SHIPPUDEN: Ultimate Ninja Storm 4 |
| 470074 | Super Buckyball Tournament               |
| 471074 | PokerStars                               |
| 472074 | FIFA 21                                  |
| 473074 | Jackbox Party                            |
| 474074 | Craftopia                                |
| 475074 | Valheim                                  |
| 476074 | Knockout City                            |
| 477074 | Farming Simulator 19                     |

### User Status Object

###### User Status Structure

| Field            | Type                                                       | Description                                   |
|------------------|------------------------------------------------------------|-----------------------------------------------|
| content          | ?[user status content object](#user-status-content-object) | the actual content of the status              |
| customReactionId | ?integer                                                   | the emoji's id that goes alongside the status |
| customReaction?  | [emoji object](/resources/team#team-emoji-object)          | the emoji that goes alongside the status      |

###### Example User Status

```json
{
  "content": {
    "object": "value",
    "document": {
      "data": {},
      "nodes": [
        {
          "data": {},
          "type": "paragraph",
          "nodes": [
            {
              "leaves": [
                {
                  "text": "g.gg/guilded-api",
                  "marks": [],
                  "object": "leaf"
                }
              ],
              "object": "text"
            },
            {
              "data": {
                "reaction": {
                  "id": 294745,
                  "customReaction": {
                    "id": 294745,
                    "name": "blobcouple",
                    "png": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/CustomReaction/448cff53087b93e72298bae9d47708f1-Full.webp?w=120&h=120",
                    "webp": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/CustomReaction/448cff53087b93e72298bae9d47708f1-Full.webp?w=120&h=120",
                    "apng": null
                  },
                  "customReactionId": 294745
                }
              },
              "type": "reaction",
              "nodes": [
                {
                  "leaves": [
                    {
                      "text": ":blobcouple:",
                      "marks": [],
                      "object": "leaf"
                    }
                  ],
                  "object": "text"
                }
              ],
              "object": "inline"
            },
            {
              "leaves": [
                {
                  "text": "g.gg/blob-emoji ",
                  "marks": [],
                  "object": "leaf"
                }
              ],
              "object": "text"
            },
            {
              "data": {
                "reaction": {
                  "id": 294677,
                  "customReaction": {
                    "id": 294677,
                    "name": "blobpats",
                    "png": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/CustomReaction/ff03be6efe491bc934b1c217d70da9ce-Full.webp?w=120&h=120",
                    "webp": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/CustomReaction/ff03be6efe491bc934b1c217d70da9ce-Full.webp?w=120&h=120",
                    "apng": null
                  },
                  "customReactionId": 294677
                }
              },
              "type": "reaction",
              "nodes": [
                {
                  "leaves": [
                    {
                      "text": ":blobpats:",
                      "marks": [],
                      "object": "leaf"
                    }
                  ],
                  "object": "text"
                }
              ],
              "object": "inline"
            }
          ],
          "object": "block"
        }
      ],
      "object": "document"
    }
  },
  "customReactionId": 294765,
  "customReaction": {
    "id": 294765,
    "name": "ablobwobwork",
    "png": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/CustomReaction/d7cf013a4d01460a81186e65f1f8c12a-Full.webp?w=120&h=120&ia=1",
    "webp": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/CustomReaction/d7cf013a4d01460a81186e65f1f8c12a-Full.webp?w=120&h=120&ia=1",
    "apng": null
  }
}
```

![example user status in the client](/images/example_status.png)

### User Status Content Object

Status content is an example of "stacked" content, much like message data. It's pretty difficult to portray its structure in a table, so I've simply not done that. The useful part of the object is the `document.nodes[0].nodes` list. Refer to [this example](#example-user-status) and parse it as you will.

### Flair Object

Represents a profile flair. Does not include stonks, those can be found at [`user.stonks`](#user-object).

###### Flair Structure

| Field  | Type    | Description                                                            | Known Values |
|--------|---------|------------------------------------------------------------------------|--------------|
| flair  | string  | name of the flair                                                      | `'gil_gang'` |
| amount | integer | quantity of this flair that the user has, probably intended for stonks | |

### Me Object

###### Me Structure

| Field           | Type                                                    | Description                                                              |
|-----------------|---------------------------------------------------------|--------------------------------------------------------------------------|
| teams           | array of [teams](/resources/team#team-object)           | a list of this user's teams                                              |
| user            | [user](#user-object)                                    | this user                                                                |
| updateMessage   | ?[update message](#update-message-object)               | guilded's update message                                                 |
| customReactions | array of [emojis](/resources/emoji#custom-emoji-object) | a list of emojis this user has access to                                 |
| reactionUsages  | array of [emoji uses](#emoji-use-object)                | a list of how many times a specific emoji has been used by the this user |
| friends         | array of [friends](#friend-object)                      | a list of friends, friend requests and friend requests sent by this user |

### Friend Object

###### Friend Structure

| Field        | Type                                   | Description                                         |
|--------------|----------------------------------------|-----------------------------------------------------|
| friendUserId | [user id](/reference/user#user-object) | id of the user                                      |
| friendStatus | string                                 | the current [status of friendship](#friend-status)  |
| createdAt    | ISO8601 timestamp                      | when the request was sent/when request got accepted |

###### Example Friend

```json
{
    "friendUserId": "R40Mp0Wd",
    "friendStatus": "accepted",
    "createdAt": "2021-01-01T15:00:00.000Z"
}
```

###### Friend Status

| Value     | Meaning                                |
|-----------|----------------------------------------|
| accepted  | the friend request was accepted        |
| requested | this user has sent a friend request    |
| pending   | a friend request was sent to this user |

### Emoji Use Object

###### Emoji Use Structure

| Field | Type    | Description                            |
|-------|---------|----------------------------------------|
| id    | integer | ID of the emoji                        |
| total | integer | how many times the emoji has been used |

###### Example Emoji Use

```json
{
    "id": 90025666,
    "total": 984
}
```

### Update Message Object

###### Update Message Structure

| Field         | Type              | Description                                 |
|---------------|-------------------|---------------------------------------------|
| id            | unsigned integer  | ID of the update message                    |
| content       | message content   | the content of the update message           |
| createdAt     | ISO8601 timestamp | when the post was created                   |
| updatedAt     | ISO8601 timestamp | when the post was edited                    |
| publishedAt   | ISO8601 timestamp | when the post was published to everyone     |
| ctaButtonText | string            | the text of the footer button               |
| ctaButtonLink | url               | the link of the footer button               |

## Get Current User
<span class="http-verb">GET</span><span class="http-path">/me</span>

Returns the [me](#me-object) object of the requester's account.

###### Query Params

| Field   | Type    | Description                                  | Required | Default Value |
|---------|---------|----------------------------------------------|----------|---------------|
| isLogin | boolean | whether or not you are logging in (probably) | false    | false         |

## Get User
<span class="http-verb">GET</span><span class="http-path">/users/{[user.id](#user-object)}</span>

Returns a [user](#user-object) object for a given user ID.

## Modify Current User
<span class="http-verb">PUT</span><span class="http-path">/users/{[user.id](#user-object)}/profilev2</span>

Modify the requester's user account settings. Returns a [user](#user-object) object on success.

!!! info
    All parameters to this endpoint are optional.

###### JSON Params

| Field     | Type                                  | Description                           |
|-----------|---------------------------------------|---------------------------------------|
| name      | string                                | new username                          |
| avatar    | string                                | new avatar from url                   |
| subdomain | string                                | new subdomain (profile url/slug)      |
| aboutInfo | [aboutInfo object](#aboutInfo-params) | new data about your profile           |

###### aboutInfo Params

!!! info
    All parameters here are nullable. When posting this to the API, this should go inside of a dictionary passed to [Modify Current User](#modify-current-user).

| Field   | Type   | Description                                                  |
|---------|--------|--------------------------------------------------------------|
| bio     | string | the user's bio ("About")                                     |
| tagLine | string | the user's tagline (appears under their name on the profile) |

## Leave Team
<span class="http-verb">DELETE</span><span class="http-path">/teams/{[team.id](/resources/team#team-object)}/members/{[user.id](#user-object)}</span>

Leave a team. Returns an empty dictionary on success.

## Get User DMs
<span class="http-verb">GET</span><span class="http-path">/users/{[user.id](#user-object)}/channels</span>

Returns a list of [DM channel](/resources/channel#dm-channel-structure) objects on success.

## Create DM Channel
<span class="http-verb">POST</span><span class="http-path">/users/{[user.id](#user-object)}/channels</span>

!!! info
    `user.id` should be the current user's ID, **not** the user who you are starting a DM with.

!!! warning
    You should not use this endpoint to DM everyone in a server. DMs should generally be initiated by a user action. For notifying a large number of users, consider a role mention.

Returns a `{"channel": dm_channel_object}`, where [dm_channel_object](/resources/channel#dm-channel-structure) is the DM channel that was created.

###### JSON Params

| Field | Type  | Description                                                                                          |
|-------|-------|------------------------------------------------------------------------------------------------------|
| users | array | array of `{"id": user.id}`, where [user.id](#user-object) is a user's id to create a DM channel with |

## Get User Posts
<span class="http-verb">GET</span><span class="http-path">/users/{[user.id](#user-object)}/posts</span>

!!! bug
    This endpoint returns an empty list.

Get the list of posts this user has on their profile.

###### Query Params

| Field    | Type    | Description                                   | Required | Default Value |
|----------|---------|-----------------------------------------------|----------|---------------|
| maxPosts | integer | the maximum amount of posts to return (0-???) | false    | 7             |
| offset   | integer | ?                                             | false    | 0             |

## Set User Transient Status
<span class="http-verb">POST</span><span class="http-path">/users/me/status/transient</span>

Set your transient status. Returns your new [transient status object](#transient-status-object) on success.

###### JSON Params

| Field  | Type     | Description                                                                         |
|--------|----------|-------------------------------------------------------------------------------------|
| id     | integer  | ?                                                                                   |
| gameId | ?integer | the game's id this status is for (see [game ids](#game-ids) for known valid values) |
| type   | string   | the type of transient status. should be one of `gamepresence`, ?                    |

## Delete User Transient Status
<span class="http-verb">DELETE</span><span class="http-path">/users/me/status/transient</span>

Remove your transient status.

## Get Referral Statistics
<span class="http-verb">GET</span><span class="http-path">/users/me/referrals</span>

Get your statistics for "stonks". From the client:

> For every 5 people you invite, you will get an exclusive Stonks flair to show off to everyone.

"Stonks" don't do anything on their own, and are simply a profile decoration.

###### Response Structure

| Field                | Type    | Description                                                                                        |
|----------------------|---------|----------------------------------------------------------------------------------------------------|
| referrals            | integer | how many people have used your invite link (https://guilded.gg?r={user.id}) to sign up for guilded |
| stonks               | integer | how many "stonks" you currently have                                                               |
| requiredForNextStonk | integer | how many more people need to sign up with your invite link for you to gain another "stonk"         |

###### Example Response

```json
{
  "referrals": 10,
  "stonks": 2,
  "requiredForNextStonk": 5
}
```
