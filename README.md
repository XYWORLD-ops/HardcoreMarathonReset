# HardcoreMarathonReset – Complete Beginner Setup Guide

This walks you from "nothing installed" to a running hardcore-marathon server, on **Windows**
(with short **Mac** notes). No prior server experience needed. Total time: about 15 minutes.

> **Honesty note:** this plugin was smoke-tested on a headless Paper 1.21.4 server (it loads,
> runs the countdown, creates/deletes worlds, persists the attempt counter and timer, all with
> zero errors). It has **not** yet been played with a live Minecraft client. Please test on a
> private server first and report anything odd via GitHub Issues.

---

## Step 1 – Install Java 21

Paper 1.21.4 needs Java 21.

1. Go to <https://adoptium.net/temurin/releases/?version=21>
2. Pick **Operating System: Windows**, **Architecture: x64**, **Package Type: JDK**, version **21**.
3. Download the **`.msi`** installer and run it. Click *Next* through the defaults.
   On the "Custom Setup" page, make sure **"Set JAVA_HOME variable"** and **"Add to PATH"** are enabled.
4. Verify: open **Command Prompt** (press `Win`, type `cmd`, Enter) and run:
   ```
   java -version
   ```
   You should see something like `openjdk version "21.0.x"`. If it says a different version or
   "not recognized", reboot and try again, or re-run the installer.

**Mac:** download the `.pkg` for macOS (aarch64 for Apple Silicon, x64 for Intel), install it,
then run `java -version` in Terminal.

---

## Step 2 – Download Paper 1.21.4 and make a server folder

1. Create a folder, e.g. `C:\MinecraftServer` (Mac: `~/MinecraftServer`).
2. Go to <https://papermc.io/downloads/all>, choose version **1.21.4**, and download the latest
   build. Save it into your folder and **rename it to `paper.jar`**.
3. Create the start script in the same folder.

   **Windows – `start.bat`:** open Notepad, paste this, then *File → Save As*, set
   "Save as type" to *All Files*, and name it `start.bat`:
   ```bat
   @echo off
   java -Xms2G -Xmx4G -jar paper.jar --nogui
   pause
   ```

   **Mac – `start.sh`:** paste this into a file named `start.sh`:
   ```sh
   #!/bin/sh
   cd "$(dirname "$0")"
   java -Xms2G -Xmx4G -jar paper.jar --nogui
   ```
   Then in Terminal: `chmod +x ~/MinecraftServer/start.sh`

   `-Xmx4G` gives the server 4 GB of RAM. If your PC has 8 GB total, use `-Xmx3G`.

---

## Step 3 – First run, EULA, server.properties

1. Double-click `start.bat` (Mac: `./start.sh`). It will download files, print a message about
   the EULA, and stop. This is normal.
2. Open the new file **`eula.txt`** in Notepad, change `eula=false` to **`eula=true`**, save.
3. Open **`server.properties`** in Notepad and set these lines (search with `Ctrl+F`):
   ```properties
   hardcore=true
   difficulty=hard
   online-mode=true
   allow-nether=true
   ```
   `hardcore=true` gives you the hardcore hearts and the "You died" hardcore screen.
   `online-mode=true` means only legitimate Minecraft accounts can join (keep it on).
4. Run `start.bat` again. Wait until you see **`Done (x.xxxs)! For help, type "help"`**.
   The window that stays open is the **server console** – you can type commands into it.
5. Type `stop` and press Enter to shut it down cleanly before the next step.

---

## Step 4 – Install the plugin

1. Open the **Releases** page of this repo:
   <https://github.com/XYWORLD-ops/HardcoreMarathonReset/releases>
2. Under the latest release, download **`HardcoreMarathonReset-1.0.0.jar`** (the server plugin – required).
3. Put it inside the **`plugins`** folder in your server folder (Paper created it on first run).
4. Start the server again with `start.bat`. In the console you should see:
   ```
   [HardcoreMarathonReset] Enabling HardcoreMarathonReset v1.0.0
   [HardcoreMarathonReset] HardcoreMarathonReset enabled. Current attempt: #1 | world: world
   ```
   The plugin also creates `plugins/HardcoreMarathonReset/config.yml` (settings you can edit)
   and `data.yml` (attempt counter + timer, managed automatically).

---

## Step 5 – Make yourself an operator and learn the commands

In the **server console** type (replace with your exact Minecraft name):
```
op YourMinecraftName
```
Now you can use these in-game (or from the console without the `/`):

| Command | What it does |
|---|---|
| `/hreset status` | Show attempt number, current world, reset state and timer |
| `/hreset now` | Start the 10-second countdown and reset the world right now (no death needed) |
| `/hreset cancel` | Stop a running countdown |
| `/hreset reload` | Reload `config.yml` after editing it |
| `/hreset timer pause` | Freeze the run timer |
| `/hreset timer resume` | Un-freeze the run timer |
| `/hreset timer reset` | Set the timer back to `00:00:00` (keeps the current world) |
| `/hreset setattempt <n>` | Set the attempt counter, e.g. `/hreset setattempt 119` |

---

## Step 6 – Join your server

1. Open the Minecraft Launcher, choose the **Java Edition 1.21.4** release, and click *Play*.
2. *Multiplayer → Add Server*, Server Address: **`localhost`**, click *Done*, then join.
3. You'll see a boss bar at the top of the screen reading **`ATTEMPT 1:  00:00:05`** – that's
   the attempt counter and run timer.
4. Test it: type `/hreset now` in chat. You'll see the red title, a 10 → 1 countdown with ticks,
   then be teleported to a brand-new world with an empty inventory and `ATTEMPT 2` on the HUD.
   From here on, **any player dying** triggers the same reset for everyone.

---

## Step 7 (optional) – Top-left HUD like the streamer overlay

A vanilla client can only show the HUD as a boss bar, sidebar, or action bar. For the big
**`ATTEMPT 119:` / `00:22:52`** text in the **top-left corner**, players install a small
client-side mod. It's optional – players without it just see the boss bar.

1. Install **Fabric Loader** for **1.21.4**: download the installer from
   <https://fabricmc.net/use/installer/>, run it, pick Minecraft version **1.21.4**, click *Install*.
2. Download the **Fabric API** mod for 1.21.4 from <https://modrinth.com/mod/fabric-api>.
3. Download **`HardcoreMarathonHUD-1.0.0.jar`** from this repo's Releases page.
4. Put both `.jar` files in your **`.minecraft/mods`** folder:
   * Windows: press `Win+R`, type `%appdata%\.minecraft\mods`, Enter (create `mods` if missing).
   * Mac: `~/Library/Application Support/minecraft/mods`
5. In the Minecraft Launcher, select the new **"fabric-loader-1.21.4"** profile and play.
   When you join the server, the top-left HUD appears and the boss bar is hidden for you.

The mod is purely visual. It only receives the attempt/timer data from the server.

---

## Step 8 – Solo or with friends

**Solo:** nothing to configure. You die → countdown → fresh world. The timer pauses while you're
offline (`timer.pause-when-empty: true` in `config.yml`).

**Friends on the internet:** they need to reach your PC. Two options:

* **Port forwarding (free, more setup):** log in to your router, forward **TCP port 25565** to
  your PC's local IP, and give friends your public IP (search "what is my ip"). Guides for your
  router model are on <https://portforward.com>.
* **playit.gg (easiest):** install the agent from <https://playit.gg>, add a *Minecraft Java*
  tunnel pointing to `127.0.0.1:25565`, and share the address it gives you. No router changes.

Friends on the **same Wi-Fi** just use your PC's local IP (e.g. `192.168.1.20`) instead of `localhost`.

---

## Troubleshooting

| Problem | Fix |
|---|---|
| `'java' is not recognized` | Java isn't on PATH. Re-run the Temurin installer and enable "Add to PATH", then reboot. |
| `UnsupportedClassVersionError` or "class file version 65" | You're running an older Java. Uninstall Java 8/17; install Temurin **21**. |
| Server stops right after starting | Open `eula.txt` and make sure it says `eula=true`. |
| `Failed to bind to port` | Another server is running on 25565. Close it, or change `server-port` in `server.properties`. |
| Plugin isn't listed / no `[HardcoreMarathonReset]` line | Make sure the jar is in `plugins/` (not `plugins/HardcoreMarathonReset/`) and you're using **Paper** (not vanilla/Forge/Fabric server). |
| `/hreset` says no permission | Run `op YourName` in the server console, then rejoin. |
| Friends can't connect | Port 25565 isn't reachable. Use playit.gg or check your port forward + Windows Firewall. |
| Short lag freeze when the world resets | Normal – the server generates three new worlds at once. It usually takes 2–8 seconds. |
| After the first reset, the old `world` folder is still there | Expected – Paper can't unload the original main world while running. Later attempts (`hcrun_N`) are deleted automatically. You can delete `world`, `world_nether`, `world_the_end` manually after the first reset. |
| Timer says `(paused)` with nobody online | Expected – `pause-when-empty` is on. It runs once a player joins. |
| Top-left HUD not showing | You must launch the **Fabric** profile, have **Fabric API** + `HardcoreMarathonHUD-1.0.0.jar` in `mods/`, and the server must have the plugin. The server log prints `<name> has the HardcoreMarathonHUD client mod` when it detects you. |
| Something else | Check the server console for a red error, then open an issue on GitHub with the log. |

Have fun, and good luck on attempt #1.
