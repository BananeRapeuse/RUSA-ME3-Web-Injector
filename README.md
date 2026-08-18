🧬 Pokémon R/S Mystery Event Injector V4.0

Inject Pokémon Ruby & Sapphire Mystery Event (.me3) data directly into save files, entirely in your browser.

100% Client-Side · No Uploads · No Server · No Installation

⸻

📖 Table of Contents

* What is this project?
* What is a Mystery Event?
* What is a .me3 file?
* Supported Games
* Features
* How to Use
* Understanding Pokémon R/S Save Files
* The Dual Save Block System
* Save Sections
* Mystery Event Section
* Mystery Event Data Location
* RM Data
* Mystery Event Activation Flag
* Checksums
* How Injection Works
* Save Format Detection
* Diagnostic Mode
* Privacy & Security
* Browser Compatibility
* Mobile Support
* Common Problems
* Mini FAQ
* Technical Reference
* Project Structure
* Limitations
* Safety & Backups
* Version

⸻

🔎 What is this project?

Pokémon R/S Mystery Event Injector is a browser-based utility for injecting Pokémon Ruby and Pokémon Sapphire Mystery Event data into compatible save files.

The project was designed around a simple idea:

You should not need a Windows-only application just to inject a Mystery Event into a Pokémon R/S save.

Instead, the entire operation happens locally in the browser.

You provide:

Pokémon Ruby/Sapphire save
        +
Mystery Event .me3 file
        ↓
R/S Mystery Event Injector
        ↓
Modified .sav

The application automatically handles:

* Save format detection
* Save block detection
* Save section inspection
* .me3 validation
* Mystery Event data injection
* Optional Mystery Event activation
* RM data injection
* Section checksum recalculation
* Both save copies
* Output save generation

⸻

🎁 What is a Mystery Event?

A Mystery Event was a special event system used by Pokémon Ruby and Pokémon Sapphire.

It allowed certain external event data to be transferred into the game.

Depending on the event, Mystery Event functionality could be used for special distributions and event-related content.

This system is separate from the later:

* Mystery Gift systems
* Wonder Card systems
* Generation IV Mystery Gift infrastructure
* Generation V Mystery Gift infrastructure

The R/S Mystery Event system uses its own save data structure.

This project specifically targets that structure.

⸻

📦 What is a .me3 file?

A .me3 file contains Mystery Event data intended for Pokémon Ruby/Sapphire.

The injector currently accepts two .me3 layouts:

File size	Contents
1004 bytes	Mystery Event data
1012 bytes	Mystery Event data + 8-byte RM data

The first 1004 bytes are treated as the main Mystery Event payload.

If the file contains an additional 8 bytes, they are treated as RM data.

Example

JIRACHI.me3

may contain the binary data required for a specific Mystery Event.

The injector does not need to understand the event’s Pokémon, item, text, or internal meaning.

It primarily needs to place the binary payload at the correct location in the save.

⸻

🎮 Supported Games

Fully targeted

* Pokémon Ruby
* Pokémon Sapphire

The project is specifically designed around the save architecture used by these games.

⸻

❌ Not supported

This injector should not be used with:

* Pokémon Emerald
* Pokémon FireRed
* Pokémon LeafGreen
* Pokémon Diamond
* Pokémon Pearl
* Pokémon Platinum
* Pokémon HeartGold
* Pokémon SoulSilver
* Pokémon Black
* Pokémon White
* Pokémon Black 2
* Pokémon White 2
* Pokémon X
* Pokémon Y
* Pokémon Omega Ruby
* Pokémon Alpha Sapphire

Those games use different save structures and event systems.

⸻

✨ Features

💾 Save support

* Standard 128 KiB R/S saves
* 16-byte wrapped save files
* .sav
* .dsv input

📦 Mystery Event support

* 1004-byte .me3
* 1012-byte .me3
* Optional 8-byte RM payload

🧬 Save processing

* Detects both save blocks
* Inspects all 14 sections per block
* Detects Mystery Event section
* Detects Flags section
* Validates section checksums
* Recalculates modified checksums

🚩 Event activation

Optional automatic Mystery Event flag activation.

🔬 Diagnostics

Detailed structural information can be displayed before injection.

📱 Browser-first

Works directly in modern browsers, including mobile browsers.

🔒 Private

Files are processed locally and are never uploaded.

⸻

🚀 How to Use

Step 1: Select your save

Choose your Pokémon Ruby/Sapphire save file.

Accepted extensions:

.sav
.dsv

The application automatically analyzes the file.

A valid save should display:

Status: Valid R/S

⸻

Step 2: Select your .me3

Choose your Mystery Event file.

For example:

JIRACHI.me3

The application checks its size.

Valid:

1004 bytes
1012 bytes

Invalid:

Anything else

⸻

Step 3: Choose whether to activate the event

The option:

Enable Event Flag Automatically

is enabled by default.

When enabled, the injector modifies the appropriate Mystery Event flag in the save.

This is generally the intended behavior when preparing a save for an event.

⸻

Step 4: Inject

Click:

💉 INJECT MYSTERY EVENT

The injector then processes both save blocks.

⸻

Step 5: Download

After successful processing:

💾 DOWNLOAD MODIFIED SAVE FILE

The generated file is downloaded as:

Pokemon_Sapphire_Event_Injected.sav

⸻

🧠 Understanding Pokémon R/S Save Files

This is the important bit if you want to understand what the injector is actually doing.

A Pokémon Ruby/Sapphire save is not simply one giant collection of data.

The save contains two copies of the save data.

These are commonly referred to as:

Block A
Block B

Each block contains:

14 sections

Each section is:

0x1000 bytes

Therefore:

14 × 0x1000
= 0xE000 bytes

Each block is therefore:

0xE000 bytes
= 57,344 bytes

The complete save core is:

0x20000 bytes
= 131,072 bytes

⸻

🔄 The Dual Save Block System

The save core can be represented approximately as:

0x00000
│
├── Block A
│   ├── Section 0
│   ├── Section 1
│   ├── Section 2
│   ├── Section 3
│   ├── Section 4  ← Mystery Event
│   ├── ...
│   └── Section 13
│
├── Block B
│   ├── Section 0
│   ├── Section 1
│   ├── Section 2
│   ├── Section 3
│   ├── Section 4  ← Mystery Event
│   ├── ...
│   └── Section 13
│
└── Remaining save space

The exact game save logic uses section metadata and counters to determine which copy is current.

For this reason, the injector deliberately modifies both copies.

⸻

🧩 Save Sections

Every block contains 14 sections.

Each section is exactly:

0x1000 bytes

Important metadata is located near the end of each section.

Offset	Size	Purpose
0x0000	0xFF4	Section data
0x0FF4	2 bytes	Section ID
0x0FF6	2 bytes	Checksum
0x0FF8	4 bytes	Reserved / metadata
0x0FFC	4 bytes	Save counter

The injector uses these fields to identify and validate sections.

⸻

🧬 Mystery Event Section

The Mystery Event section has:

Section ID = 4

The injector searches every save block for section ID 4.

This means it does not blindly assume the section is always present at one hardcoded address.

If it finds the section, its actual location is used.

If it cannot identify the section, the injector falls back to the expected section position.

⸻

📍 Mystery Event Data Location

Inside the Mystery Event section, the main event payload begins at:

0x810

Therefore:

Mystery Event Address
=
Mystery Event Section Address
+
0x810

For example:

Section address = 0x4000
0x4000 + 0x810
= 0x4810

The injector performs this calculation separately for Block A and Block B.

⸻

📦 RM Data

Some .me3 files contain an additional 8-byte payload.

The project calls this:

RM Data

When present, it is written at:

Mystery Event Section + 0xBFC

Therefore:

RM address
=
Mystery Event Section
+
0xBFC

If the .me3 is only 1004 bytes, there is no RM payload to copy.

⸻

🚩 Mystery Event Activation Flag

Injecting the event data and enabling the corresponding event flag are separate operations.

V4.0 provides an option to automatically enable the flag.

The relevant Flags section is:

Section ID = 2

The injector modifies:

Flags Section + 0x3A9

and enables:

bit 0x10

The operation is performed using:

existing byte OR 0x10

This is important because the injector does not overwrite the entire byte.

It only enables the requested bit while preserving the other bits.

⸻

🧮 Checksums

Save sections contain checksums.

Whenever the injector changes section data, the old checksum is no longer valid.

Therefore the injector recalculates the checksum.

The checksum covers:

0x0000 → 0x0FF3

which is:

0xFF4 bytes

The data is processed as 32-bit little-endian values.

Conceptually:

sum = word0
    + word1
    + word2
    + ...

The result is folded:

sum += sum >> 16

and the lower 16 bits become the stored checksum.

⸻

❗ Why Checksums Matter

If you modify save data without updating its checksum, the section can become invalid.

That can cause:

* Save corruption
* Data being ignored
* The game selecting another save copy
* Unexpected behavior

V4.0 therefore recalculates the checksum after modifying:

1. The Mystery Event section
2. The Flags section, if the activation flag is enabled

⸻

🔁 How Injection Works

For each save block:

1. Find Mystery Event section
          ↓
2. Copy 1004-byte ME payload
          ↓
3. Copy optional 8-byte RM payload
          ↓
4. Recalculate ME section checksum
          ↓
5. Find Flags section
          ↓
6. Enable flag if requested
          ↓
7. Recalculate Flags section checksum

This happens twice:

Block A
   +
Block B

⸻

🗃️ Save Format Detection

V4.0 recognizes two main layouts.

Standard save

0x20000 bytes

The save core starts at:

offset 0

⸻

16-byte wrapped save

0x20010 bytes

The application treats the first:

16 bytes

as a wrapper/header.

The actual save core begins at:

offset 0x10

The wrapper is preserved when generating the output.

⸻

🔬 Diagnostic Mode

The diagnostic tool is useful when you are unsure whether a save is structurally compatible.

Click:

🔬 RUN DETAILED DIAGNOSTIC

The application reports information such as:

Save Size
Save Format
Data Offset
Block A
Block B
Mystery Event Section
Event File Status

It can also report how many sections have valid checksums.

Example:

Block A: 14/14 sections checked
Block B: 14/14 sections checked

⸻

🩺 Common Problems

“Invalid Format”

The selected save could not be recognized as a supported R/S save.

Possible causes:

* Wrong game
* Wrong file
* Unsupported wrapper
* Corrupted file
* Emulator-specific format
* Incorrect save size

First verify that the save really comes from:

Pokémon Ruby

or:

Pokémon Sapphire

⸻

“Expected size: 1004 or 1012 bytes”

The selected event file is not a supported .me3 size.

Check the file size.

Supported:

1004 bytes
1012 bytes

⸻

The file is .sav, but it is rejected

The .sav extension does not determine the internal format.

Two files can both be called:

save.sav

while containing completely different save structures.

The injector checks the binary structure rather than trusting the extension alone.

⸻

The event is injected but does not appear in-game

First verify:

* The save is actually Ruby/Sapphire.
* The correct .me3 was used.
* The output save was copied to the correct location.
* The save was actually loaded by the emulator/device.
* The event activation flag was enabled if required.
* The save was not subsequently overwritten by another save copy.

The injector modifies the save binary. It does not control how a particular emulator, flashcart, or hardware device imports that save.

⸻

The game says the save is corrupted

Do not continue using the modified file.

Restore the original backup first.

Then investigate the compatibility of the original save format.

Never rely on a modified save as your only copy.

⸻

❓ Mini FAQ

What is a Mystery Event?

A Mystery Event is an event-distribution mechanism used by Pokémon Ruby and Pokémon Sapphire.

It allows specific external event data to be stored in the game’s save.

⸻

What is .me3?

.me3 is a binary Mystery Event file format associated with the Ruby/Sapphire Mystery Event system.

V4.0 accepts 1004-byte and 1012-byte files.

⸻

Can I use this with Pokémon Emerald?

No.

Emerald has a different save/event structure.

This injector is specifically designed for Ruby and Sapphire.

⸻

Can I use a .sav from an emulator?

Potentially, yes, provided that the file contains a compatible Ruby/Sapphire save structure.

The extension alone is not enough.

⸻

Does the injector upload my save?

No.

The file is processed locally inside your browser.

⸻

Does it require an Internet connection?

The application itself does not require a server or online processing.

Once the HTML application is available locally, the actual injection process is performed locally.

⸻

Does it modify my original save?

No.

The original input is copied into a separate output buffer.

The modified file is downloaded separately.

⸻

Why are both save blocks modified?

Because Pokémon Ruby/Sapphire maintain two copies of their save data.

The injector updates both copies so that the Mystery Event data exists consistently in the duplicated save structure.

⸻

Why are checksums recalculated?

Because changing section data invalidates the old checksum.

The checksum must therefore be updated after modification.

⸻

What happens if I disable “Enable Event Flag Automatically”?

The Mystery Event data is still injected.

However, the automatic flag modification is skipped.

This is useful when you specifically want to control the flag yourself.

⸻

What is RM data?

Some .me3 files contain an additional 8-byte payload after the main 1004-byte event data.

V4.0 copies those 8 bytes to the corresponding RM location when present.

⸻

Can I inject multiple events?

The injector is designed around injecting one supplied .me3 payload per operation.

If multiple events need to be prepared, process the save according to the intended event workflow and keep backups of each stage.

⸻

Does the injector modify Pokémon data directly?

No.

It does not function as a general Pokémon save editor.

Its purpose is specifically to inject Mystery Event data and optionally enable the associated event flag.

⸻

Can I use it to edit Pokémon, items, money, trainer data, etc.?

No.

Those are outside the scope of this project.

⸻

Does the tool decrypt anything?

No.

The application works directly on the supplied save bytes.

It does not implement a separate encryption/decryption layer.

⸻

Can I run it on iPhone?

Yes, provided the browser supports the required File API and local file download behavior.

The interface is responsive and specifically designed to work on small screens.

⸻

Can I run it on Android?

Modern browsers with the required Web APIs should generally be suitable.

⸻

Does it require Windows?

No.

The application is browser-based.

It can therefore be used on:

* Windows
* macOS
* Linux
* iOS
* iPadOS
* Android

provided the browser supports the required APIs.

⸻

🧪 Technical Reference

Save constants

SAVE_CORE      = 0x20000
SAVE_WITH_16   = 0x20010
BLOCK          = 0xE000
SECTOR         = 0x1000
DATA           = 0xFF4
ID             = 0xFF4
CS             = 0xFF6
CTR            = 0xFFC

⸻

Section identifiers

Mystery Event Section = 4
Flags Section         = 2

⸻

Mystery Event offsets

ME Data = 0x810
RM Data = 0xBFC

⸻

Event flag

Flags Section + 0x3A9
Bit = 0x10

⸻

Section layout

+0x0000
│
│ Section Data
│ 0xFF4 bytes
│
+0x0FF4
│ Section ID
│
+0x0FF6
│ Checksum
│
+0x0FF8
│ Metadata / Reserved
│
+0x0FFC
│ Save Counter
│
+0x1000

⸻

🧭 Complete Binary Workflow

A simplified representation of the operation is:

                INPUT
                  │
        ┌─────────┴─────────┐
        │                   │
        ▼                   ▼
     .SAV/.DSV            .ME3
        │                   │
        ▼                   ▼
  Detect format        Validate size
        │                   │
        ▼                   ▼
  Extract core        Split ME / RM
        │                   │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │     BLOCK A       │
        │                   │
        │ Find Section 4    │
        │ Write ME          │
        │ Write RM          │
        │ Update checksum   │
        │                   │
        │ Find Section 2    │
        │ Set flag          │
        │ Update checksum   │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │     BLOCK B       │
        │                   │
        │ Find Section 4    │
        │ Write ME          │
        │ Write RM          │
        │ Update checksum   │
        │                   │
        │ Find Section 2    │
        │ Set flag          │
        │ Update checksum   │
        └─────────┬─────────┘
                  │
                  ▼
        Rebuild original file
                  │
                  ▼
             DOWNLOAD

⸻

🔒 Privacy & Security

The project intentionally follows a local-first architecture.

There is no:

* Account system
* Database
* Upload endpoint
* API
* Cloud save storage
* Server-side binary processing

The browser reads the selected files directly.

The binary data remains in browser memory while processing.

The output is generated locally with a Blob and downloaded through the browser.

Data flow

Your Device
    │
    ├── Save File
    │
    ├── .me3 File
    │
    ▼
Browser Memory
    │
    ├── Validate
    ├── Inspect
    ├── Modify
    └── Recalculate
    │
    ▼
Modified Save
    │
    ▼
Your Device

There is no intentional network transfer of the save or event file.

⸻

📱 Mobile Support

The interface is designed with mobile devices in mind.

The UI includes:

* Responsive cards
* Large touch targets
* Mobile-friendly file pickers
* Drag & drop where supported
* Responsive statistics
* Safe-area support
* No desktop-only dependencies

On screens below 600 pixels wide, the information grid automatically switches to two columns.

⸻

🧑‍💻 Running the Project

The project is intentionally self-contained.

No build system is required.

No dependencies need to be installed.

Save the HTML source as:

index.html

Then open it in a compatible browser.

That’s it.

⸻

🌍 Hosting

Because everything is client-side, the application can be hosted as a static website.

The server only needs to deliver:

index.html

There is no backend requirement.

This makes the project compatible with practically any static hosting environment.

⸻

🛡️ Backup Recommendations

Always keep the original save.

Before using the injector:

Original Save
    │
    ├── Backup #1
    ├── Backup #2
    └── Working Copy
             │
             ▼
          Injector
             │
             ▼
       Modified Save

Never overwrite your only copy of a save.

For important or irreplaceable Pokémon/event saves, keep multiple backups.

⸻

⚠️ Limitations

V4.0 intentionally has a narrow scope.

It is not a general Pokémon save editor.

It does not attempt to:

* Repair arbitrary corrupted saves
* Convert every emulator save format
* Edit Pokémon structures
* Edit trainer data
* Edit inventory
* Edit Pokédex data
* Generate Mystery Events
* Decode every field of the .me3
* Support Emerald’s different save system
* Support later-generation Mystery Gift systems

The project focuses on one task:

Safely placing compatible R/S Mystery Event data into a compatible Ruby/Sapphire save structure.

⸻

🧠 Design Philosophy

The project is built around four principles.

1. Local-first

Your save should stay on your device.

2. Transparent

The binary operations should be understandable and inspectable.

3. Portable

A browser should be enough.

4. Non-destructive

The original file should not be overwritten during processing.

⸻

🏗️ Project Architecture

R/S Mystery Event Injector
│
├── UI
│   ├── Save uploader
│   ├── ME3 uploader
│   ├── Event activation checkbox
│   ├── Injection button
│   ├── Diagnostic button
│   ├── Result panel
│   └── Process log
│
├── Save Engine
│   ├── Format detection
│   ├── Block detection
│   ├── Section inspection
│   └── Checksum validation
│
├── Mystery Event Engine
│   ├── ME3 validation
│   ├── ME extraction
│   ├── RM extraction
│   └── Injection
│
├── Flag Engine
│   ├── Flags section detection
│   └── Event flag activation
│
├── Integrity Engine
│   └── Checksum recalculation
│
└── Output Engine
    └── Modified save generation

⸻

🧬 Main Internal Functions

detectSave(bytes)

Detects the supported save layout and extracts the save core.

⸻

inspectBlock(bytes, offset)

Inspects all 14 sections of a save block and identifies relevant sections.

⸻

detectME(bytes)

Validates the .me3 file and separates its ME and optional RM payloads.

⸻

sectionChecksum(bytes, offset)

Calculates the checksum for a save section.

⸻

diagnostic()

Displays structural information about the currently loaded save.

⸻

inject()

Performs the actual Mystery Event injection and generates the modified output.

⸻

📊 Quick Reference

Question	Answer
What games?	Pokémon Ruby & Sapphire
What event format?	.me3
ME3 sizes?	1004 / 1012 bytes
Save size?	128 KiB core
Wrapped save?	128 KiB + 16 bytes
Save blocks?	2
Sections per block?	14
Section size?	0x1000
Mystery Event section?	ID 4
Flags section?	ID 2
ME offset?	0x810
RM offset?	0xBFC
Flag offset?	0x3A9
Flag bit?	0x10
Checksums updated?	Yes
Both blocks modified?	Yes
Server required?	No
Uploads files?	No
Installation required?	No
Mobile compatible?	Yes, with a compatible browser

⸻

🆘 Troubleshooting Checklist

If something goes wrong, check these in order:

* The game is Pokémon Ruby or Pokémon Sapphire.
* The save is a compatible R/S save.
* The save is not truncated.
* The .me3 file is exactly 1004 or 1012 bytes.
* The correct event file is being used.
* The event flag option is enabled if required.
* The generated save was downloaded completely.
* The generated save was copied to the correct emulator/device location.
* The original save has been preserved as a backup.

⸻

📜 Version History

V4.0

Major browser-based release featuring:

* Client-side save processing
* R/S save format detection
* 128 KiB save support
* 16-byte wrapper support
* Dual-block processing
* Section validation
* Mystery Event section detection
* .me3 validation
* ME payload injection
* RM payload injection
* Automatic Mystery Event flag activation
* Checksum recalculation
* Diagnostic mode
* Drag & drop support
* Responsive mobile UI
* Local output generation

⸻

📄 License

Add the project’s chosen license here.

If no license has been selected yet, this section should be updated before publishing the repository.

⸻

🧬 Final Summary

Pokémon R/S Mystery Event Injector V4.0 is a specialized, browser-based tool for working with the original Mystery Event save structure of Pokémon Ruby and Pokémon Sapphire.

It combines:

Save analysis
      +
Mystery Event injection
      +
Flag activation
      +
Checksum repair
      +
Dual-block synchronization
      +
Client-side privacy
      =
Portable R/S Mystery Event tooling

The goal is not to replace a complete Pokémon save editor.

The goal is to make R/S Mystery Event injection simple, transparent, portable and accessible, including on devices where traditional desktop tools are unavailable.

Select the save. Select the .me3. Inject. Download. 🧬🎮
