# How to Turn a Fire TV Stick into a Retro Gaming Console (Using ADB on Mac)

*Published June 2026 · Tech · Tutorial · 12 min read*

---

A step-by-step guide for setting up RetroArch on an older Fire TV Stick using ADB from your Mac — no remote required.

---

## What You'll Need

- An Amazon Fire TV Stick (Fire OS 7 or 8 — **not** a 2025/2026 Vega OS model)
- A Mac with Homebrew installed
- Your iPhone or Android phone (to use as a remote)
- A Bluetooth controller (Xbox or PlayStation)
- A monitor or TV with an HDMI port

> **Note:** This guide does not work on the newer Fire TV Stick 4K Select or Fire TV Stick HD (2026), which run Vega OS and do not support sideloading.

---

## Step 1: Check Your Fire Stick Model

Plug in the Fire Stick and go to **Settings → My Fire TV → About**. Confirm you see **Fire OS 7 or Fire OS 8** in the Software Version. If you see Vega OS, stop here — sideloading is not supported.

---

## Step 2: Set Up Your Phone as a Remote

If you don't have a physical remote, download the **Amazon Fire TV** app on your phone (free on iOS and Android). Make sure your phone and Fire Stick are on the same WiFi network. Open the app and pair it to your Fire Stick — it will show a 4-digit code on screen to confirm.

---

## Step 3: Enable Developer Options

1. Go to **Settings → My Fire TV → About**
2. Click on your Fire TV device name **7 times rapidly**
3. You'll see a message: *"You are now a developer"*
4. Go back to **My Fire TV** and open the new **Developer Options** menu
5. Turn on **ADB Debugging**
6. Turn on **Apps from Unknown Sources** (or "Install Unknown Apps")

---

## Step 4: Install ADB on Your Mac

Open Terminal and run:

```bash
brew install android-platform-tools
```

If you get a permissions error first, run:

```bash
sudo chown -R $(whoami) /usr/local/share/info /usr/local/share/man/man3 /usr/local/share/man/man5
```

Then retry the install command.

---

## Step 5: Find Your Fire Stick's IP Address

On the Fire Stick go to **Settings → My Fire TV → About → Network** and note the IP Address (e.g. `192.168.1.218`).

---

## Step 6: Connect ADB to Your Fire Stick

In Terminal run:

```bash
adb connect 192.168.1.218:5555
```

(Replace with your actual IP address.)

Your Fire Stick will show an authorization popup — select **Allow**. If you see `already connected` or `connected to 192.168.1.218:5555` you're good to go.

To reconnect in a future session, just run the same command again.

---

## Step 7: Check Your Fire Stick's Architecture

Before downloading any emulator, check what processor your Fire Stick uses:

```bash
adb shell getprop ro.product.cpu.abilist
```

- If it returns `arm64-v8a` — you have a **64-bit** processor (great, more options available)
- If it returns `armeabi-v7a` — you have a **32-bit** processor (older device, use 32-bit APKs)

---

## Step 8: Install RetroArch

Download the RetroArch APK to your Mac from [retroarch.com](https://www.retroarch.com/?page=platforms) — choose **Android (32-bit)** if your device returned `armeabi-v7a` above.

Then install it via ADB:

```bash
adb install ~/Downloads/RetroArch*.apk
```

You should see `Success` when it's done.

---

## Step 9: Install Sideload Launcher (Optional but Recommended)

Sideloaded apps don't appear on the Fire TV home screen by default. Search for **Sideload Launcher** in the Amazon Appstore (~$4) to give yourself easy access to RetroArch without needing Terminal every time.

---

## Step 10: Connect a Bluetooth Controller

Put your controller into pairing mode (PS5: hold **PS + Create** simultaneously until the light bar flashes rapidly). Then on the Fire Stick go to:

**Settings → Controllers & Bluetooth Devices → Other Bluetooth Devices → Add Bluetooth Devices**

---

## Step 11: Download Cores in RetroArch

Open RetroArch and go to **Online Updater → Core Downloader**. For SNES games (recommended starting point), download:

**Nintendo - SNES / SFC (Snes9x)**

Other useful cores:
- **Nintendo - Game Boy Advance (mGBA)** — GBA games
- **Nintendo - Nintendo 64 (ParaLLEl N64)** — N64 games (64-bit devices only)

---

## Step 12: Add ROM Files

Once you have ROM files on your Mac, push them to the Fire Stick with:

```bash
adb push ~/Downloads/"Game Name (USA).sfc" /sdcard/ROMs/
```

To check what's already on the device:

```bash
adb shell ls /sdcard/ROMs/
```

To delete a file:

```bash
adb shell 'rm "/sdcard/ROMs/filename.sfc"'
```

---

## Step 13: Play a Game

In RetroArch, go to **Load Content → /storage/emulated/0 → ROMs**, select your game, choose the appropriate core, and hit **Run**.

**PS5 controller tip:** The **Options button** acts as Start, and the **Create button** acts as Select for most SNES games.

---

## Games That Work Great on 32-bit Fire Sticks

All of these run on the Snes9x core:

- Kirby Super Star
- Donkey Kong Country 1, 2 & 3
- Super Mario World
- Super Mario World 2: Yoshi's Island
- Teenage Mutant Ninja Turtles IV: Turtles in Time
- Street Fighter II Turbo
- Aladdin
- The Legend of Zelda: A Link to the Past

---

## Limitations

- **3DS games** (like Kirby: Planet Robobot) require a 64-bit processor **and** a Snapdragon 835 or better. Most Fire Sticks don't qualify.
- **N64 games** are hit or miss on older 32-bit devices — they may crash depending on the title.
- **Wii/GameCube/PS2** are not realistic on any Fire Stick hardware.

For 3DS emulation, consider installing **Azahar** on a Mac, or picking up a **Retroid Pocket 5** from Target.

---

*Will Harris is an independent consultant and occasional weekend tinkerer. He can be reached at [will@harrisconsulting.us](mailto:will@harrisconsulting.us).*
