# GT4 Multiplayer

## Open Source Release 0.18.30

This is the confirmed working online race launcher made for the Gran Turismo 4 USA Online Beta.

It includes the launcher, PCSX2, the local online services and the source code.

It does not include a game image, PS2 BIOS, memory card, account, ZeroTier network, save, log or personal file.

You need your own legally dumped Gran Turismo 4 USA Online Beta ISO and your own PS2 BIOS BIN file.

## Installation

1. Extract the whole release ZIP to a normal folder
2. Keep the Setup EXE beside the Payload folder
3. Run Gran Turismo Bulgaria Open Source Setup.exe
4. Select your own Gran Turismo 4 USA Online Beta ISO
5. Select your own PS2 BIOS BIN file
6. Choose an empty installation folder and install
7. The installer checks the app, checks your files and prepares your own ISO for this online setup. Your original file is not changed

## ZeroTier setup

1. Install ZeroTier on every computer that will play
2. Create your own private network in ZeroTier Central
3. Open ZeroTier Setup in the launcher and enter your own Network ID
4. Join that same Network ID on every computer
5. Authorize every computer in ZeroTier Central
6. Wait until the launcher says ZeroTier is ready
7. The person hosting must log into GT Online first before the person joining tries to log in

## Local router setup

1. Put every computer on the same trusted home router
2. Choose Local Router LAN in the launcher on every computer
3. No ZeroTier network is needed for this option

## How to host

1. Choose ZeroTier or Local Router LAN
2. Select Host Match
3. Create the invite and copy it
4. Launch the host game
5. Go into Online and register a new Gran Turismo account if you do not already have an account on this local server
6. Log into GT Online and wait at the online menu
7. Only after the host is logged in should the joiner continue

## How to join

1. Use the same connection type as the host
2. For ZeroTier make sure the computer is authorized on the same private ZeroTier network
3. Select Join Match
4. Paste the complete invite from the host
5. Launch the joiner game
6. Go into Online and register a new Gran Turismo account on this server
7. Log in after the host is already online

## Known DNAS timeout

Sometimes the DNAS authentication may take 2 times when you click Online because of the known timeout issue. Wait for it to take you back to the menu then try a second time. If it still does not work close the game and app and restart.

## Important

Every computer must use the same release and the supported Online Beta image.

Do not reuse an account name at the same time on two computers.

Only authorize people you trust on your private ZeroTier network.

Closing the launcher and game stops the local online services started by that launcher.

This is an unofficial community project and is not connected with Sony, Polyphony Digital or ZeroTier.
