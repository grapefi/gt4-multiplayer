# Gran Turismo Online Technicals

This document explains the complete working path from installing the project to finishing an online race and returning to the Gran Turismo 4 Online menu.

It describes what the game expects at every stage and what the GT4 Multiplayer app supplies so the game can continue. It contains no personal account names, private addresses, device identities, network IDs, file paths, passwords, invitation keys, logs, or signing secrets.

## Supported game build

The complete flow is built for one exact disc image.

| Requirement | Expected value |
| --- | --- |
| Game | Gran Turismo 4 USA Online Public Beta |
| Disc ID | SCUS 97436 |
| Game version | 1.00 |
| ISO size | 2,654,109,696 bytes |
| Original ISO SHA256 | `1f49b9d8077801188860b62c3efdbc631ba8af7df979373811ce8c60dcc48ebe` |
| Prepared ISO SHA256 | `675622dbb4a4036c03b51a3f8ac00cccdc1cf1a665586e625869dafd45f960bc` |

The normal USA v2.00 retail game, other regions, Japanese online test releases, and custom combined images do not use the same executable, memory layout, DNAS modules, or network protocol profile.

## Service map

These are fixed application services and not personal network details.

| Service | Transport | Purpose |
| --- | --- | --- |
| Private launcher session | TCP on the invitation port | Authenticates the host and joining launchers and carries control messages |
| DNAS compatibility service | TCP 443 | Handles the original PlayStation 2 network authentication stage |
| NAT service | UDP 10070 | Returns the reachable private endpoint used by the session |
| MUIS | TCP 10071 | Supplies the universe, lobby, account, friend, room, and matchmaking entry point |
| DME | TCP 10073 | Creates the live game world and coordinates players entering it |
| MAS | TCP 10075 | Authenticates the DME server with application 10782 |
| GT4 race traffic | UDP 6000 | Carries the native Online Beta race packets through the selected adapter |

The host runs the compatible service stack. Joining computers connect to those services through either the shared ZeroTier network or the same trusted physical router.

# Complete progress through one online race

## Step 1: Downloading the release

### What the game needs

The game itself does not run yet. The future installation needs an intact launcher, PCSX2 runtime, backend, DNAS service, assets, and configuration templates that all belong to the same release.

### What the app supplies

The release ZIP supplies the installer, signed Payload folder, launcher, PCSX2 runtime, backend runtime, DNAS compatibility service, interface assets, documentation, and required third party notices.

The release does not supply a game ISO, PlayStation 2 BIOS, memory card, account database, save, ZeroTier network, or any private user information.

### What allows progress

The installer executable must remain beside the complete Payload folder. Missing or modified signed files stop installation before user owned game files are copied.

## Step 2: Verifying and importing the ISO

### What the game needs

The runtime hooks and backend behavior require the exact SCUS 97436 Online Public Beta build. A filename is not enough because any ISO can be renamed.

### What the app supplies

The installer checks the exact file size and computes the complete SHA256 hash. It accepts the verified original image or the exact previously prepared image.

When the original image is selected, the installer copies it into the new installation and changes only the ten verified DNAS compatibility bytes. The original user owned ISO is never edited.

### What allows progress

The copied image must finish with the prepared SHA256 hash shown above. If it does not, installation stops instead of launching an unknown build.

## Step 3: Verifying and importing the BIOS

### What the game needs

PCSX2 needs a valid PlayStation 2 BIOS to boot the game.

### What the app supplies

The installer accepts a user selected BIOS BIN file and checks that it is the expected 4 MiB format. The BIOS is copied into the isolated PCSX2 data area used by this installation.

### What allows progress

PCSX2 receives the BIOS location in its generated configuration. The project never downloads or distributes a BIOS.

## Step 4: Verifying the installed application

### What the game needs

Every launcher, backend, and configuration file must match the release that was tested with the game.

### What the app supplies

The installer verifies the signed release manifest before copying the Payload. The launcher verifies the installed manifest again before starting a game session.

### What allows progress

A missing, changed, or untrusted application file causes a clear failure before the backend or PCSX2 is started. This prevents a partly updated installation from reaching the online protocol.

## Step 5: Selecting ZeroTier or Local Router LAN

### What the game needs

All players need private IPv4 connectivity to the host. The game must receive addresses that are reachable by every participant.

### What the app supplies

For ZeroTier, the launcher detects the authorized ZeroTier adapter and its managed private address. Every computer must be authorized on the same private ZeroTier network.

For Local Router LAN, the launcher detects a private address on the trusted physical router. This option is only for computers on the same local network.

### What allows progress

The launcher rejects loopback, unavailable, or unsuitable addresses. The selected adapter becomes the one PCSX2 uses for both the login path and live UDP race traffic.

## Step 6: Creating the host invitation

### What the game needs

Before the game starts, every launcher must agree on one host, one network type, and one authenticated private session.

### What the app supplies

Host Match starts a private launcher session on the selected adapter. It generates a fresh 32 byte random secret and places the transport type, required network reference, host endpoint, and private session information inside the invitation.

The control connection authenticates both sides with challenge and response messages. Control data uses separate derived encryption and authentication keys. The invitation is cleared from the launcher clipboard after a short period when it has not been replaced.

### What allows progress

The host sees that the session is waiting for invited players. Only someone with both access to the private network and the complete invitation can authenticate to the launcher session.

The invitation must still be treated as private and sent only to intended players.

## Step 7: Joining the private launcher session

### What the game needs

The joining computer must use the same network path and must know which computer owns the compatible online server.

### What the app supplies

Join Match reads the invitation, selects the correct transport, checks the local adapter, and connects to the host. For ZeroTier it checks membership in the referenced network and requests membership when needed. The network owner must still authorize the joining device.

The joiner proves possession of the invitation secret. The host and joiner derive new control channel keys rather than sending those keys directly.

### What allows progress

The app reports that the host is authenticated and the private room is ready. If authentication, authorization, or private routing fails, PCSX2 is not launched into an empty session.

## Step 8: Starting and checking the host backend

### What the game needs

The original official servers are no longer available. GT4 needs replacements for DNAS, NAT, account login, universe selection, rooms, matchmaking, and the DME game world.

### What the app supplies

The host launcher starts the bundled DNAS and Horizon services on the selected private address. It configures the backend for GT4 application 10782 and starts the required DNAS, NAT, MUIS, MAS, and DME components.

The launcher checks that DNAS is listening on TCP 443, NAT on UDP 10070, MUIS on TCP 10071, DME on TCP 10073, and MAS on TCP 10075. It also checks that MAS and DME completed their internal authentication.

### What allows progress

Only after all required services are ready does the host launcher send the authenticated server ready message to joined launchers.

The joiner then probes the host service ports. If they cannot be reached, the joiner stops before starting the game and reports that the private network or Windows Private network access needs attention.

## Step 9: Building the isolated PCSX2 profile

### What the game needs

PCSX2 needs a BIOS, graphics settings, controller mapping, network device, memory card area, PINE monitoring port, and the exact prepared ISO.

### What the app supplies

The launcher creates or updates an isolated PCSX2 data directory for that player. It copies the imported BIOS support files, applies graphics settings, assigns the selected controller, enables the PINE interface, and selects a unique monitoring port.

It verifies the Online Beta image again before launch and starts PCSX2 directly with the prepared image.

### What allows progress

A second GT4 process is blocked from using the same player data at the same time. This prevents two emulator processes from fighting over one memory card, network configuration, or monitoring port.

## Step 10: Configuring the emulated PS2 network

### What the game needs

GT4 expects a PlayStation 2 Ethernet adapter with DHCP, DNS, and a stable route for both TCP services and UDP race packets.

### What the app supplies

The launcher enables PCSX2 DEV9 Ethernet in Sockets mode and binds it to the selected ZeroTier or physical LAN adapter. It enables DHCP interception, uses PCSX2 internal DNS, keeps automatic DNS as a fallback, and reserves UDP 6000 for the Online Beta race profile.

It maps the retired Online Beta service names to the authenticated host address. The host and joiner therefore ask for the original service names while PCSX2 sends them to the private replacement service.

### What allows progress

Inside GT4, the selected network configuration should use automatic IP address, automatic DNS, and no proxy. If GT4 asks to create a network configuration, use any generic name such as **GT4 Multiplayer** and keep those values automatic. The profile name itself does not affect connectivity.

The network test succeeds when the emulated PS2 receives an address and can resolve the original service names through PCSX2.

## Step 11: Booting the exact game runtime

### What the game needs

The runtime monitor must see the executable layout for SCUS 97436 with the expected Online Beta CRC and network profile.

### What the app supplies

The launcher starts a read only build check through PINE. DNAS compatibility hooks only recognize the verified instructions and addresses from this build.

### What allows progress

Unknown instructions or a different game build are not modified. The launcher records a build mismatch and stops unsafe memory writes.

## Step 12: Entering Online and performing DNAS authentication

### What the game needs

GT4 loads its DNAS module, connects to the DNAS hostname on TCP 443, completes the legacy TLS exchange, and waits for an authenticated status before opening the Online Beta login path.

### What the app supplies

PCSX2 internal DNS directs the DNAS hostname to the host. The replacement DNAS service supplies the expected regional certificate and protocol responses.

The prepared image selects the known DNAS compatibility route. The exact build monitor also has narrow fallbacks for the verified DNAS module copies and authentication state when the original game times out while consuming the reply.

### What allows progress

The terminal DNAS state becomes authenticated and GT4 advances to the online service screens.

A known timing issue can make the first DNAS attempt return to the menu. The correct action is to wait for that return and select Online again. If the second attempt fails, close the game and launcher and restart the host first sequence.

## Step 13: Loading the Online Beta service entry pages

### What the game needs

After DNAS, GT4 requests the original service entry pages, server information, terms text, and Online Beta configuration.

### What the app supplies

The compatible SVO and Horizon routes answer the retired service requests and direct GT4 toward the private MUIS service for application 10782.

### What allows progress

GT4 displays the login or registration interface instead of a dead official server page or a permanent black screen.

## Step 14: Registering or logging into an account

### What the game needs

GT4 needs a unique account record, a successful account authentication response, a session key, and an active Medius client session.

### What the app supplies

The host backend stores accounts created on that server installation and answers registration and login requests. No personal account database is shipped in the release.

### What allows progress

Each computer uses a separate account name. After successful login, the backend associates the account with its current private endpoint and returns the universe and lobby information.

The host must finish this step before joiners log in. That order ensures the host owned service and its first live session already exist.

## Step 15: Entering the default lobby

### What the game needs

GT4 requests available channels, player lists, friend state, chat routing, and game lists.

### What the app supplies

MUIS and the Medius lobby server create or return the default lobby, maintain connected client objects, provide player and room lists, and forward supported chat and friend messages.

### What allows progress

Both players appear online and can enter the same waiting room. The server keeps each account tied to the correct authenticated session rather than allowing one connection to replace the other.

## Step 16: Creating or finding a race

### What the game needs

For a custom race the host sends the room name, rules, track, car restrictions, capacity, and security information. For Quick Race the game submits matchmaking filters and waits for a compatible game.

### What the app supplies

The backend accepts the original Medius create game or match create game request, records the room under application 10782, and returns it through the game list and matchmaking responses.

### What allows progress

The host receives a successful game creation response. Joiners can see the room or receive the matching game through the original menus.

## Step 17: Requesting entry to the race

### What the game needs

The joining client sends a Medius join game request for the selected race. The server must confirm that the account, room, capacity, and session are valid.

### What the app supplies

The backend resolves the selected game and asks DME to place the client into the same game world. The confirmed race join corrections preserve the reachable private address instead of returning the emulator only PS2 address that caused the old reboot error.

### What allows progress

The join response contains the correct DME world, private endpoint, and connection information. The joiner advances into the same pre race area as the host.

## Step 18: NAT discovery and endpoint confirmation

### What the game needs

The Online Beta asks the NAT service what address and source port the other side can actually reach. The result must match the selected session adapter.

### What the app supplies

The NAT component listens on UDP 10070 and returns the observed private IPv4 address and source port. PCSX2 keeps the login connection and the UDP probe on the same selected adapter so Windows does not send them through different networks.

### What allows progress

Every player receives reachable private endpoints. No public internet port forwarding is needed when ZeroTier is used, and the joiner never needs router port forwarding.

## Step 19: Creating the DME game world

### What the game needs

The host race needs a DME world ID. Every accepted participant must be registered in that same world before live race traffic begins.

### What the app supplies

MAS authenticates the bundled DME service for application 10782. DME creates the world, accepts the server join requests, and reports successful placement back through Medius.

### What allows progress

The host and joiners all hold the same world ID and can advance from the waiting room into the race loading sequence.

## Step 20: Synchronizing the pre race state

### What the game needs

Every client needs matching track, race rules, car restrictions, participant order, and ready state. All clients must remain at normal emulation speed.

### What the app supplies

The backend keeps the host as the authority for the room configuration and forwards the original lobby state messages. The launcher configures normal EE timing, normal frame pacing, and the NTSC 59.94 Hz profile instead of an uncapped emulator speed.

### What allows progress

Every player reaches ready state with the same race configuration. Starting with different releases, images, or timing settings is treated as a compatibility problem rather than something the backend can safely repair during loading.

## Step 21: Loading the track

### What the game needs

Each computer loads its own track and car assets from the same game image. Network sockets must remain open during the period when little traffic is exchanged.

### What the app supplies

PCSX2 continues using the selected adapter and UDP 6000 mapping while the backend keeps the DME world and authenticated launcher session alive.

### What allows progress

Every client finishes loading and confirms entry to the same race world. If one image or game state is incompatible, the race should fail before normal driving rather than silently combining different data.

## Step 22: Running the live race

### What the game needs

During driving, each game instance simulates its own car and sends the Online Beta race packets needed to represent participants to the other clients. Stable latency, packet order, and full emulation speed are important.

### What the app supplies

The launcher does not replace GT4 physics. It keeps PCSX2 bound to the chosen private adapter and keeps the authenticated host services available. The Online Beta native UDP traffic travels over ZeroTier or the local router on the verified race profile.

### What allows progress

Each client receives the other cars and advances the same race session. The server remains responsible for session and world membership while GT4 handles the actual driving simulation and display.

## Step 23: Crossing the finish line

### What the game needs

GT4 must keep the race session alive long enough to finalize positions, lap data, times, and the local result screen. The game then sends its original player, world, and end game reports.

### What the app supplies

The backend accepts the Medius player reports, world reports, and end game report for the active world. DME keeps the world valid until the game completes its end sequence.

### What allows progress

The checkered flag, finishing order, and result screen appear normally. Players should not close PCSX2, disconnect ZeroTier, or close the host launcher while results are still being processed.

The confirmed working release preserves the original Online Beta result flow. It does not claim that every original public leaderboard service has been recreated unless that feature is explicitly added and tested in a later release.

## Step 24: Leaving the completed race

### What the game needs

After the result screen GT4 leaves the active DME game world and requests the correct room or lobby state.

### What the app supplies

The backend processes the leave game and end game state, removes the player from the completed world when appropriate, and keeps the account logged in. The lobby recovery logic can move a client back to the default lobby when the original game does not send a distinct leave channel packet while backing out.

### What allows progress

The player returns to the online race menu, waiting room, or default lobby and can create or join another race without restarting the whole application.

## Step 25: Returning to the main game menu

### What the game needs

GT4 needs orderly logout or channel leave responses if the player exits Online entirely.

### What the app supplies

MUIS removes the active account and channel membership when the game logs out or disconnects. The host backend remains available for other authenticated players until the host closes the session.

### What allows progress

GT4 returns to its normal main menu. A player can re enter Online while the host session is still alive, following the same DNAS and login stages.

## Step 26: Closing the game and app

### What the game needs

No further service is required after PCSX2 exits and the private session is finished.

### What the app supplies

The launcher closes its owned backend processes, private session sockets, monitors, and temporary control state. Sensitive invitation and session key material is cleared from active objects when the session is disposed.

### What allows progress

The private GT4 services stop listening when the app is closed. A later match starts a fresh authenticated launcher session and performs the readiness checks again.

# Normal successful order

1. Every computer installs the same release with the supported SCUS 97436 image and its own BIOS
2. Every computer connects to the same authorized ZeroTier network or trusted local router
3. The host selects Host Match and creates a fresh invitation
4. The joiner pastes the complete invitation and authenticates to the host launcher
5. The host launcher starts and verifies DNAS, NAT, MUIS, MAS, and DME
6. The joiner verifies that the host services are reachable
7. The host launches GT4 and logs into Online first
8. The joiner launches GT4 and logs in only after the host reaches the Online menu
9. Both players enter the same lobby or matched room
10. The host creates the race and the joiner enters it
11. NAT and DME give both players the same reachable private race world
12. Every player becomes ready with matching race data
13. The track loads and live UDP race traffic begins
14. The players complete the race and wait through the result screen
15. The backend accepts the end game flow
16. The game returns everyone to the room or online menu
17. Players may start another race or leave Online normally
18. Closing the host launcher ends the private services and session

# What the app deliberately does not supply

1. It does not supply or download the game ISO
2. It does not supply or download a PlayStation 2 BIOS
3. It does not include anyone's account database or password
4. It does not include a memory card or personal save
5. It does not include a ZeroTier Network ID or device identity
6. It does not publish the host services directly to the public internet
7. It does not bypass private network authorization
8. It does not replace GT4 driving physics with a new game engine
9. It does not accept an unknown ISO and attempt unsafe runtime writes
10. It does not guarantee unimplemented public leaderboard or campaign features

# Information that must remain private

1. The complete host invitation
2. The private ZeroTier Network ID when the owner does not intend to publish it
3. Account passwords
4. Support archives and logs until they have been reviewed and sanitized
5. Private release signing keys
6. Any real public or private address connected to a particular person

Use placeholders such as `<NETWORK ID>`, `<INVITATION>`, `<ACCOUNT NAME>`, and `<HOST ADDRESS>` in public instructions, screenshots, examples, and bug reports.
