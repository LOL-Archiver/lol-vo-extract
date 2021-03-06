# lol-vo-extract
League of Legends Champion Voices Download/Extract/Rename with events.

Most of implementation based on [CDTB](https://github.com/CommunityDragon/CDTB)'s by Node.js 10.X+. Sincerely Thanks!

## ATTENTION PLEASE
`lol-vo-extract` does not guarantee 100% match for all events for all skin:
- Because different skins have **different** production dates and packaging formats, it is very easy to extract out unknown events.
- Sometimes the literal and actual trigger are different!

Some not perfect match as following:
* Lux: Not all events from `skin*.BIN` and `_event.bnk` can match. Need to match manually through `data/EventIDMap/Lux.json`;
* Twist Fate: `VO` and `SFX` are not independent of each other. Need to increase `sfxLevel`.
* Mecha Kingdoms Jax: The event `JaxRelentlessAssault` shows that the trigger of this is to activate the passive `Relentless Assault`. But in the actual game, the trigger is to use the R skill `Grandmaster's Might`.

## Installation

`lol-vo-extract` requires `Node.js v10.X+` or higher for ES2015, Async function and BigInt support. (v8.X+ maybe OK, v6.X+ should be not)

Install NPM Package before use:
````javascript
npm install
````

NOTE: `lol-vo-extract` depend on [xxhash](https://github.com/mscdex/node-xxhash). It is a C library that need to rebuild when installing. If rebuild failed, consider install [Windows-Build-Tools](https://github.com/felixrieseberg/windows-build-tools) at first.

## Usage
The entry file is `index.js`. All extracted files will be stored in `_final` folder:
````batch
cd lol-vo-extract
node index
````

`lol-vo-extract` use `config.js` to determine everything. `config.default.js` is sample.

````javascript
module.exports = {
	region: 'NA1',
	solution: 'game',
	cdn: 'https://lol.dyn.riotcdn.net',
	sie: 'https://sieve.services.riotcdn.net',

	proxy: false,
	// proxy: { host: '127.0.0.1', port: 9000, },

	champ: 'annie',
	lang: 'en_us',
	format: 'wav',

	// 0, no SFX; 1, only asset; 2, all
	useSFXLevel: 0,
	// 0, cache first; 1, force fetch; 2, use client file
	useWADLevel: 0,

	detect: {
		baseForce: false,

		array: [],

		min: 0,
		max: 27,
	},

	path: {
		rextractorConsole: 'D:/RavioliGameTools/RExtractorConsole.exeRExtractorConsole.exe',
		game: 'D:/Game/League of Legends',
	}
};
````

## useSFXLevel
`VO` and `SFX` are two types of sound that each skin has. They are usually independent of each other. But some skins use each other.
We can choose to parse SFX events and audios by modifying the `useSFXLevel` option in `config.js`.
* Level 0, no SFX events and sounds;
* Level 1, use SFX events;
* Level 2, use SFX events and extract SFX audios;

## Convert WEM to WAV/OGG

Currently, `lol-vo-extract` can extract `WEM` directly without `RExtractor`.

`lol-vo-extract` also support convert `WEM` to `WAV` or `OGG` by calling `RExtractorConsole.exe`. You need to configure the path of `rextractorConsole` at first.

````javascript
module.exports = {
	// ...
	rextractorConsolePath: 'D:/RavioliGameTools/RExtractorConsole.exe',
	// ...
};
````