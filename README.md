# Sims 3 Water Planes

A step-by-step guide to creating and adding water planes in **The Sims 3 Create a World**, using SandrineD’s generator, Notepad++, and S3PE.

A companion to **aspnull’s CAW tutorial**: create a lake above sea level, then add another one later. Includes download links and credits.

## Downloads

1. **[SandrineD’s generator](https://modthesims.info/showthread.php?t=453392&page=1&pp=25)** — download **707.rar from post #25**. Extract it and open `707gen.html` in your browser. Keep `signature.jpg` beside it.
2. **[Notepad++](https://notepad-plus-plus.org/downloads/)** — install **HEX-Editor** and **Converter** through **Plugins → Plugins Admin** if they are missing.
3. **[S3PE — SourceForge download](https://sourceforge.net/projects/sims3tools/files/latest/download)** — or at **[Simlogical](http://www.simlogical.com/ContentUploadsRemote/uploads/189/)**

> You do not need the Bridgeport files or HxD for this method.

## 1. Prepare your lake

1. Back up your world.
2. Sculpt a basin with raised banks and a clear, low center.
3. Place trees at diagonally opposite corners. Write down their **Position X and Z**.
4. Check the **Position Y** of the bottom and the banks.

Enter these in SandrineD’s generator:

| Field | What to enter |
| --- | --- |
| **X1** | Smaller X, rounded down |
| **Z1** | Smaller Z, rounded down |
| **X2** | Larger X, rounded up |
| **Z2** | Larger Z, rounded up |
| **Altitude** | Water height: above the bottom, below the banks |

Compare the two X values and the two Z values **separately**. X1 and Z1 may come from different trees.

Use a rectangle **more than 32 units wide and long** for this method. Leave the other settings at default and generate the hex.

**Save and close CAW.**

## 2. Make the water-plane file

1. Copy the generated hex numbers.
2. In Notepad++, open a **new ordinary text tab** and paste.
3. Select all, then choose **Plugins → Converter → HEX → ASCII**.
4. Choose **Plugins → HEX-Editor → View in HEX**. Check that the bytes match the generator’s numbers.
5. Choose **Save As → All types**, using this exact filename:

```text
S3_707CA200_00000000_0000000000000000%%+UNKN.bnry
```

Copy the filename exactly for this method. S3PE reads the resource type, group, and instance from it when importing.

- `%%+UNKN` is S3PE naming metadata. It is **UNKN**, not UKN.
- `.bnry` is the binary resource file extension.
- The final long number is the **instance**. The first plane uses **0**.

> If saving returns to text view, that is okay. **Do not convert it again.**

## 3. Set the plane count

1. Open your `.world` file in **S3PE**.
2. Find Type **`0x90624C1B`**.
3. Select it, then choose **Resource → Export → To file**.
4. Open the exported file in Notepad++ and enable **View in HEX**.
5. At **row `00000010`, column `2`**, change **`00` to `01`**. Replace the byte; do not insert a new one.
6. Save.

This is offset **`0x12`**: the third byte on that row when there are 16 bytes per row.

The total is now **one plane**. The sea does not count.

## 4. Import and check

1. In S3PE, choose **Resource → Import → From file**.
2. Select your new **707 `.bnry`** and edited **906 `.bnry`**. Do not select `.bak` files.
3. Check **Replace duplicates** and **Use resource name**. Import, save, and close S3PE.
4. Open the world in CAW.
5. Make a small terrain change near the bank, save, close, and reopen. Check that the water stays.

Shape the shoreline with terrain, keeping the ground at the plane’s center **below the water surface**. Save and reopen again after sculpting.

## Add a second plane

Repeat the measurements and file creation, but save the new plane with instance **1**:

```text
S3_707CA200_00000000_0000000000000001%%+UNKN.bnry
```

1. Save the world and close CAW.
2. Export the **current 906 resource** from your world.
3. Change the same count byte from **`01` to `02`**, then save.
4. Import the **new 707** and **updated 906**. Leave the first plane in place.
5. Save and close S3PE, then reopen the world in CAW. Repeat the terrain-change, save, and reopen check to confirm both remain.

**First plane = instance 0. Second plane = instance 1. Total count = 02.**

## Credits

- **SandrineD** — [707 generator, post #25](https://modthesims.info/showthread.php?t=453392&page=1&pp=25).
- **zaide_chris** — [original water-plane tutorial and research](https://modthesims.info/t/453392).
- **simsample** — [instructions and troubleshooting](https://modthesims.info/showthread.php?t=602551).
- **FLSTRider** — [illustrated water-plane tutorial](https://forums.ea.com/discussions/the-sims-3-creative-corner-en/water-plane-tutorial---hopefully-lol---pictures-are-working-again/771161).
- **[S3PE](https://sourceforge.net/projects/sims3tools/files/s3pe/), [Notepad++](https://notepad-plus-plus.org/), and plugin contributors** — the editing tools used here.
- **aspnull** — video demonstration and this tested workflow.

Original tools are linked above, not included in this repository.
