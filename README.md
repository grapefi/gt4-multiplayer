# GT4 Multiplayer (Gran Turismo 4 Online)

GT4 Multiplayer is a Windows launcher and local server package for the Gran Turismo 4 USA Online Beta. It makes the original online menus usable again through either a private ZeroTier network or a trusted local router.

This project includes the launcher, PCSX2, the compatible local online services, the installer, and the source code. It does not include Gran Turismo 4, a PlayStation 2 BIOS, memory cards, accounts, saves.etc

Made it to play GT4 with a Bulgarian friend hence the name, had some fun & hope you enjoy it as well.

There is also a read me explaining exactly how it works so you can recreate this yourself if you want to.

## What you need

1. A 64 bit Windows 11 computer
2. Your own legally dumped Gran Turismo 4 USA Online Beta ISO
3. Your own PlayStation 2 BIOS BIN file
4. A controller connected before starting the game
5. ZeroTier installed on every computer for play across different internet connections
6. The same GT4 Multiplayer release on every computer

You need your own copy of Gran Turismo 4 USA Online Public Beta ISO:
- Disc ID: SCUS-97436
- Version: 1.00
- Exact size: 2,654,109,696 bytes
Use the original ISO. The installer automatically makes and patches its own copy without modifying your original.

For two computers on the same trusted home router you can use Local Router LAN and do not need ZeroTier.

## Download

Open the [latest release](https://github.com/grapefi/gt4-multiplayer/releases/latest) and download this file:

**Gran.Turismo.Bulgaria.Open.Source.Online.Beta.v0.18.30.zip**

The separate source archive is only needed if you want to inspect or build the project yourself.

## Install the game launcher

1. Right click the downloaded ZIP and select Extract All
2. Open the extracted folder named gtbos-public-stage
3. Keep **Gran Turismo Bulgaria Open Source Setup.exe** beside the **Payload** folder
4. Run **Gran Turismo Bulgaria Open Source Setup.exe**
5. Select your own Gran Turismo 4 USA Online Beta ISO when asked
6. Select your own PlayStation 2 BIOS BIN file when asked
7. Choose a new empty installation folder
8. Finish the installation and open Gran Turismo Bulgaria

The installer verifies the signed application payload and checks the complete ISO hash. If you select the supported original dump it creates a prepared copy for this online setup. Your original ISO is not changed.

## Choose how you want to connect

### ZeroTier for different internet connections

Use this when the players are in different homes or on different networks.

1. Install ZeroTier on every computer that will play
2. Sign in to [ZeroTier Central](https://central.zerotier.com/)
3. Create a new private ZeroTier network
4. Copy the 16 character Network ID
5. Open ZeroTier Setup inside the GT4 Multiplayer launcher
6. Enter the Network ID and join it on every computer
7. Open the network in ZeroTier Central
8. Authorize every computer in the Members list
9. Wait until every computer has a managed ZeroTier IP address
10. Return to the launcher and wait until it reports that ZeroTier is ready

Every player must use the same ZeroTier Network ID. Only authorize computers and people you trust.

The person joining does not need port forwarding. The host also does not need to expose the old game service ports directly to the public internet because the players communicate through the private ZeroTier network.

### Local Router LAN for the same home network

Use this when every computer is connected to the same trusted router.

1. Connect every computer to the same home router
2. Open the launcher on every computer
3. Choose **Local Router LAN** as the connection type
4. Wait until the launcher reports that the local connection is ready

Do not use this mode on a public or untrusted network.

## Host an online game

The host must enter GT Online before the joining players. This order is important.

1. Connect the controller before launching the game
2. Open Gran Turismo Bulgaria
3. Choose the same connection type that everyone will use
4. Select **Host Match**
5. Create an invite and copy the complete invite text
6. Send the invite to the other player through a method you trust
7. Launch the host game
8. Wait 15-30 Seconds for the game to connect & launch
8. Open **Online** inside Gran Turismo 4
9. Register a new Gran Turismo account if this server does not already have your account
10. Log in and wait at the GT Online menu
11. Tell the joining player to continue only after the host is fully logged in
12. Create or enter the race room and wait for the other player

The Gran Turismo account is stored by that host installation. Do not use the same account at the same time on two computers.

## Join an online game

1. Connect the controller before launching the game
2. Join and authorize the same ZeroTier network as the host, or connect to the same router for Local Router LAN
3. Open Gran Turismo Bulgaria
4. Choose the same connection type as the host
5. Select **Join Match**
6. Paste the complete invite from the host
7. Confirm that the launcher reports the connection as ready
8. Wait until the host says they are logged into GT Online
9. Launch the joining game
10. Open **Online** inside Gran Turismo 4
11. Register a new and unique Gran Turismo account if needed
12. Log in and join the host through the online menus

## Starting a race

1. Confirm that both players can see each other in the online room
2. Let the host create the race
3. Let every joining player enter the race waiting area
4. Select cars and race settings normally
5. Start only after every player appears ready

The login and lobby stages use the local server running on the host. When the race starts the server gives each client the private addresses and ports needed for the race session. Time sensitive race traffic then travels through the selected ZeroTier or local network adapter.

## The known DNAS timeout

Sometimes DNAS authentication times out on the first attempt.

1. Wait for the game to return to the previous menu
2. Select Online again
3. The second attempt normally connects
4. If it still fails close both the game and launcher
5. Reopen the launcher and repeat the host first login order

Do not interrupt PCSX2 while the first attempt is returning to the menu.

## Quick troubleshooting

### The launcher says ZeroTier is not ready

1. Check that ZeroTier is running on the computer
2. Check that the correct Network ID was entered
3. Check that the device is authorized in ZeroTier Central
4. Check that the device has a managed ZeroTier IP address
5. Restart ZeroTier and then reopen the launcher

### The joiner cannot reach the host

1. Confirm both computers use the same release
2. Confirm both computers use the same connection type
3. Confirm both computers are authorized on the same ZeroTier network
4. Confirm the host launched through **Host Match**
5. Confirm the host is already logged into GT Online
6. Paste a newly created complete invite
7. Close any other VPN that could take priority over the selected adapter
8. Restart the game and launcher on both computers if the old session is still present

### DNAS fails

Wait for the first attempt to finish and try Online a second time. If it fails again restart both the game and launcher. Confirm that the host started first and that the correct network adapter is ready.

### Login fails

Use a unique account name for each computer. New installations start with no personal account database, so register a new account through the game before logging in.

### The lobby works but the race does not start

1. Confirm every computer uses the same v0.18.30 package
2. Confirm every computer uses the supported USA Online Beta ISO
3. Confirm every player entered through the same host session
4. Confirm the selected ZeroTier or local adapter is still connected
5. Return to the online menu and create a fresh room
6. If needed restart the host first and let the joiner connect after the host logs in

### The controller works in the launcher but not in the game

Connect the controller before launching PCSX2. Close other controller mapping tools that may claim the same device. Open the launcher controller settings and refresh the connected devices before starting again.

You can also press ESC in-game to bring up the PCSX2 Menu and then go to settings, Controller to adjust emulator controller settings.

## Verify the downloads

Release v0.18.30 has these SHA256 checksums:

| File | SHA256 |
| --- | --- |
| Installer package | `86aaa6e67b73fb3e82f507edcb3f730d5bb57122723545b7487fefd2084bcc42` |
| Source package | `9a3b64358b4c595ed0c8b2e8199fb34b806675b465d31e736bc114f6c9efb6a6` |

GitHub also displays these digests beside the release assets.

## Security and privacy

The release does not contain a game ISO, PS2 BIOS, memory card, save, Gran Turismo account, password, ZeroTier Network ID, support archive, gameplay log, Windows user name, personal path, or private release signing key.

The installer imports your own ISO and BIOS locally. Those files stay on your computer. Closing the launcher and PCSX2 stops the local online services started for that session.

ZeroTier access is controlled by the owner of the private ZeroTier network. Remove or deauthorize a device in ZeroTier Central when it should no longer be allowed to connect.

## How it works

The original public Online Beta services no longer exist. The package starts compatible local replacements when the host launches a match.

1. The DNAS service handles the original authentication stage
2. The Horizon service handles registration, login, friends, rooms, matchmaking, and the move into a race
3. The launcher configures PCSX2 for the selected private network adapter
4. The old service names are directed to the compatible services running on the host
5. The invite tells the joining launcher which private host session to use
6. The corrected race join path advertises a reachable private address instead of the internal PS2 address
7. PCSX2 carries the live race traffic over ZeroTier or the local router

More technical detail is available in [HOW IT WORKS.txt](HOW%20IT%20WORKS.txt). Build instructions are in [SOURCE BUILD.txt](SOURCE%20BUILD.txt). Privacy details are in [PRIVACY.txt](PRIVACY.txt).

## Important notes

Thanks to PCSX2 for making PS2/PC emulation awesome

This release supports the Gran Turismo 4 USA Online Beta only.

Every computer must use the same release and a supported matching game image.

The host must log into Online before the joiner.

Our Youtube: https://www.youtube.com/@GTBulgariaOnline

This is an unofficial community project. It is not connected with or endorsed by Sony, Polyphony Digital, PlayStation, PCSX2 or ZeroTier.
