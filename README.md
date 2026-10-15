# VideoScribe Offline Fix

## Problem

VideoScribe becomes corrupted whenever it connects to the internet.  
After corruption, reinstalling the software does not fix it. However, deleting the AppData folders listed below has been found to resolve the issue.

## Solution

Use the provided `blockinternet.bat` script to:
- Block all inbound and outbound internet access for VideoScribe executables.
- Clear the AppData folders that store corrupted cache and configuration.

Once blocked, VideoScribe can be used fully offline without getting corrupted.

## Observed IP Addresses

While monitoring network traffic, VideoScribe was seen attempting to connect to these AWS IP addresses:

- `52.216.24.6:80` (Amazon S3)
- `18.239.111.69:443` (AWS CloudFront / Global Accelerator)

These are shared AWS IPs and may change over time. Blocking them alone is not enough for a permanent fix — you must block the executable itself.

## Instructions

1. **Install and Run.**
2. **At the login section, simply click on 'Login' without filling in anything.**
3. **If VideoScribe gets corrupted, run `blockinternet.bat` while VideoScribe is open.**

## What `blockinternet.bat` Does

- Kills any running VideoScribe processes.
- Deletes these folders:
  - `%APPDATA%\VideoScribeDesktop`
  - `%LOCALAPPDATA%\VideoScribeDesktop`
  - `%APPDATA%\VideoScribeDesktop\Local Store\Library`
- Searches for `VideoScribe*.exe` on your system and adds Windows Firewall rules to block both **Inbound** and **Outbound** traffic for each executable found.
- Also blocks the two known AWS IPs (`52.216.24.6` and `18.239.111.69`).
- Must be run **as Administrator** and **while VideoScribe is open** so the correct executable path is detected.

## Usage

1. Save `blockinternet.bat` to your desktop.
2. Open VideoScribe (it can be at the login screen or fully loaded).
3. Right-click `blockinternet.bat` and select **Run as administrator**.
4. Type `Y` to confirm.
5. The script will automatically:
   - Kill VideoScribe processes.
   - Delete the AppData folders.
   - Add firewall rules for all `VideoScribe*.exe` files.
   - Block the known AWS IPs.
6. After completion, close and reopen VideoScribe. It should now work offline without corruption.

**Warning:** Backup your VideoScribe projects before running, as the script deletes AppData folders.

## Unblocking

To restore internet access, remove the firewall rules named `Block_VS_*` and `Block_IP_VS_*` from Windows Firewall.

## Notes

- The script must be run as **Administrator**.
- Run it **while VideoScribe is open** so the correct executable path is detected.
- Blocking the IPs alone is not enough; blocking the executable is required.
- For a more permanent block, consider using the hosts file or a tool like Simplewall to block domains `videoscribe.co` and `sparkol.com`.
- After blocking, VideoScribe will work in offline mode. All online features (cloud sync, image library, etc.) will be unavailable.

## License

This project is provided as-is for educational purposes. Use at your own risk.
