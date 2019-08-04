# Open Active Directory Search Window

"C:\Windows\System32\rundll32.exe" dsquery.dll,OpenQueryWindow

# BSOD when running Android Emulator after 1903 update installed

`SYSTEM_SERVICE_EXCEPTION` BSOD occurs when Android Emulator after Windows 10 1903 Update Installed.

How to fix:

1. Turn off Hyper-V
2. Turn off Windows Sandbox
3. "Disable" `Turn on Virtualization Based Security` in Local Group Policy
    1. Open `gpedit.msc`
    2. Go to `Computer Configuration` -> `Administrative Templates` -> `System` -> `Device Guard`
    3. Open `Turn on Virtualization Based Security`
    4. Set `Disable` (Even if `Not Configured` is already selected)

reference: https://www.reddit.com/r/Windows10/comments/bxcawn/windows_v1903_crashing_when_running_nox_player/
