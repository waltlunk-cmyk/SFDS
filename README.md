# Lost Adventurer Waypoint (Fabric, Minecraft 26.2)

Client-side mod for Hypixel SkyBlock Dungeons. When a **Lost Adventurer** mini-boss shows up, it
puts a glowing box and a tall beam on the spot where it was first seen. Both render through walls.

## How it works (and the one big limitation)

The Lost Adventurer has **no fixed spawn point**. Dungeon layouts are randomized every run, and a
mini-boss can appear in any of several rooms. So a waypoint can't exist before the game tells your
client the mob is there. The mod watches the entities your client has already been sent, looks for a
name tag containing "Lost Adventurer", and remembers where it first saw it (that is the spawn point).
It will appear when you get within the server's entity-tracking range of the room.

The waypoint is removed when you are within ~32 blocks and the name tag is gone (mob killed), or when
the world changes (new run), or with `/lostadv clear`.

It only reads data the client already has. It sends nothing to the server and changes no packets.

## Commands

- `/lostadv` toggles the mod on/off
- `/lostadv clear` clears current waypoints

## Easiest way to get the jar (no JDK or Gradle needed)

This project includes a GitHub Actions workflow that builds the jar for you.

1. Create a free GitHub account and a new repository.
2. Upload the contents of this folder to it (including the hidden `.github` folder).
3. Open the repository's **Actions** tab. The "Build mod jar" workflow runs on upload
   (or press **Run workflow**).
4. When it finishes, open the run and download the **lost-adventurer-waypoint-jar** artifact.
   Unzip it to get the `.jar`.
5. In the Modrinth App, open a Minecraft 26.2 + Fabric instance, add **Fabric API**, then add the jar
   as content from a file (or drop it into the instance's `mods` folder).

If the build fails, the log shows the exact compile error line.

## Build

Requires **JDK 25** and Gradle 9.x (the Fabric template ships a wrapper).

Easiest route, since I could not generate the Gradle wrapper offline:

1. Go to https://fabricmc.net/develop/template/ and generate a project for Minecraft **26.2**.
2. Copy this project's `src/` folder into it (replace the template's `src/`).
3. Make sure `fabric.mod.json` lists the `client` entrypoint from this project.
4. Run `./gradlew build`. The jar is in `build/libs/`.

Or use the included `build.gradle`, `gradle.properties` and `settings.gradle` directly if you already
have a Gradle 9 install (`gradle wrapper`, then `./gradlew build`).

Put the jar in your `mods` folder with Fabric Loader 0.19.x and Fabric API for 26.2.

## Tuning

In `LostAdventurerTracker`:
- `NAME_NEEDLE`: text matched in the name tag
- `NAMETAG_HEIGHT`: how far below the name tag the box sits
- `MERGE_RADIUS_SQ`, `CONFIRM_GONE_RADIUS_SQ`, `MAX_SPOTS`

## Heads up

- I could not compile this in my sandbox (no network access to download Minecraft). The rendering code
  follows Fabric's official 26.2 docs, but Minecraft's internal names move around between versions, so
  the first build may need a small fix. Compiler errors will point to the exact line.
- Written for 26.2 (what Hypixel recommends for SkyBlock as of mid-2026). Other versions need changes,
  especially the rendering code.
- Hypixel says all mod use is at your own risk. Check their current allowed-modifications page.
