# Comprehensive E-Reader & Library DRM Removal Guide
*A step-by-step reference guide for setting up an automated, permanent digital library on Linux Mint.*

---

## Prerequisite: Get a Free Adobe ID
Before installing anything, you must have a free Adobe identity. This identity acts as your security key to fetch books legally from public libraries (like the National Library of Greece).
*   **Sign-up URL:** [https://account.adobe.com](https://account.adobe.com)
*   *Keep your Adobe username (email) and password handy for the plugin configurations below.*

---

## Step 1: Install the Correct Version of Calibre
**CRITICAL:** Do not use the Linux Mint Software Manager (Software Center, Flatpak, or Apt) to install Calibre. These versions run in restricted environments that block DRM removal plugins from functioning.

1. Open your terminal (`Ctrl + Alt + T`).
2. Run the official binary installation script by pasting this command:
   ```bash
   sudo -v && wget -nv -O- https://download.calibre-ebook.com/linux-installer.sh | sudo sh /dev/stdin
   ```
3. Launch Calibre from your application menu once the installation completes.

---

## Step 2: Install & Configure the "ACSM Input" Plugin
This plugin bypasses Adobe's official desktop software, allowing Calibre to open `.acsm` files directly and download the underlying encrypted EPUB/PDF book.

1. In Calibre, open **Preferences** (the gear icon) and select **Plugins**.
2. Click **Get new plugins** at the bottom of the window.
3. In the search box, type: `ACSM Input`
4. Select the plugin, click **Install**, and confirm any security prompts.
5. **Restart Calibre** when prompted.
6. Go back to **Preferences** -> **Plugins** -> expand **File type plugins**.
7. Highlight **ACSM Input** and click **Customize plugin**.
8. Set the ADE version to emulate to **ADE 2.0.1** (for maximum compatibility).
9. Enter your **Adobe ID Email and Password**, then click **OK** and **Apply**.

---

## Step 3: Download & Extract the "NoDRM" (DeDRM) Plugin
Because DRM removal utilities are legally sensitive, this plugin must be downloaded manually from a trusted third-party repository.

1. Open your web browser and navigate to the official GitHub releases page:
   *   **URL:** [https://github.com/noDRM/DeDRM_tools/releases/tag/v10.0.9](https://github.com/noDRM/DeDRM_tools/releases/tag/v10.0.9)
2. Download the file named **`DeDRM_tools_10.0.9.zip`** to your Downloads folder.
3. Open your file manager, navigate to your Downloads folder, and **unzip/extract** `DeDRM_tools_10.0.9.zip`.
4. Open the newly extracted folder (`DeDRM_tools_10.0.9`). Inside, you will see a file named **`DeDRM_plugin.zip`**.
   *   *CRITICAL: Do not unzip this inner `DeDRM_plugin.zip` file. This raw zip bundle is exactly what Calibre requires.*

---

## Step 4: Install the NoDRM Plugin into Calibre

1. In Calibre, go to **Preferences** -> **Plugins**.
2. Click the **Load plugin from file** button in the bottom right corner.
3. Navigate into the unzipped folder from Step 3 and select **`DeDRM_plugin.zip`**.
4. Click **Open**, click **Yes** on the security warning, and then **Restart Calibre**.
5. *Verification:* Go to **Preferences** -> **Plugins** -> expand **File type plugins**. Ensure **DeDRM** is active and not grayed out.

---

## Step 5: Link the Plugins (Adopt Encryption Keys)
To ensure the NoDRM tool automatically breaks the lock on books downloaded via the ACSM Input plugin, they need to share security keys.

1. Go to Calibre **Preferences** -> **Plugins** -> expand **File type plugins**.
2. Highlight **DeDRM** and click **Customize plugin**.
3. In the configuration window, click on **Adobe Digital Editions ebooks**.
4. Click the button to **Adopt keys** or **Import keys** from other plugins/default paths. This pulls your saved Adobe credentials from the ACSM configuration.
5. Click **OK**, then click **Apply** to save the changes.

---

## Step 6: The Workflow — Importing and Reading

### For Books Already in Your Library (Cached/Locked)
If a book was previously added and still shows a DRM lock, Calibre’s cache must be cleared:
1. Right-click the book in Calibre and select **Remove books** -> **Remove selected books**.
2. **Close Calibre entirely** and reopen it to flush the memory.

### For New Downloads (`.acsm` files from nlg.gr)
1. Download your book ticket (`URLLink.acsm`) from the National Library of Greece portal.
2. Drag and drop `URLLink.acsm` directly into the main Calibre window.
3. **The Chain Reaction:** `ACSM Input` downloads the book using your Adobe ID -> `NoDRM` strips the protection on-the-fly -> Calibre updates the book title, metadata, and cover automatically.
4. Double-click the book to test it. If it opens smoothly in the Calibre reader, **the file is 100% unlocked and permanent.**

---

## Step 7: Transferring to Devices

### Option A: Transfer to an iPhone (Apple Books App)
Apple Books cannot read DRM-protected files, but since your file is now completely clean, it will accept it natively:
1. Right-click the unlocked book on your Calibre shelf and select **Open data folder**.
2. Copy the underlying `.epub` file to your iPhone using your preferred method (AirDrop, emailing it to yourself as an attachment, or dropping it into an iCloud Drive folder).
3. Tap the file on your iPhone and choose to open it with the native **Books** app.

### Option B: Transfer to an E-Reader (e.g., Kobo)
1. Plug your Kobo into your Linux Mint PC via its USB cable.
2. Tap **Connect** on the Kobo screen. 
   *   *Note: If Calibre doesn't see the device instantly, click the "Eject" icon next to the Kobo drive inside your Mint file manager (Nemo) to unmount it so Calibre can claim the connection.*
   *   *Tip: In your custom environment, if you use automated system zsh aliases, make sure they don't block raw hardware mounting hooks.*
3. In Calibre, right-click your book and select **Send to device** -> **Send to main memory**.

---

## Pro-Tip: Automated Kobo Optimization (KEPUB)
To make your books load faster, turn pages instantly, and show accurate chapter statistics on Kobo hardware, automate standard EPUB to KEPUB optimization:
1. Go to Calibre **Preferences** -> **Plugins** -> **Get new plugins**.
2. Search for and install **`KoboTouchExtended`**.
3. Restart Calibre. The plugin will automatically optimize your clean EPUB files during cable transfers without touching your master copies on your computer.
