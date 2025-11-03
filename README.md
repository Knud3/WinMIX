# WinMIX: Unattended Windows 11 Installation

**An automated method for installing a clean, streamlined Windows 11 experience, even on unsupported (old) hardware.**

If, like me, you are "responsible" for maintaining computers and tech for all your family, relatives, and friends, this project offers a solution to simplify the Windows 11 installation process. It provides a customized `autounattend.xml` file that automates the setup, delivering a consistent, bloat-free, starting point without relying on third-party tools that often require ongoing maintenance and are prone to failure. The solution leverages the native Windows setup mechanism for maximum stability and reliability.

## Prerequisites

- A USB drive (8 GB or larger)
  > Note that a few USB sticks out there may not support booting for some reason.

## Customization

The following items in the unattended installation file can be customized if needed:

- **Localization:** To change the localization, modify lines 86 and 93-96 in `autounattend.xml` (By default it is set to Finnish).
  > For a comprehensive list of available languages and input locales, refer to the [Microsoft documentation](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/default-input-locales-for-windows-language-packs?view=windows-11#input-locales).
- **User Account:** The default username for the initial user account can be changed on line 114.
- For more advanced modifications, refer to the generator URL provided at the end of this document.

## Usage

The process is very simple, TL;DR: Create a bootable USB drive, copy the `autounattend.xml` file to it, and boot from the drive.

1. Use the official [Windows 11 Media Creation Tool](https://www.microsoft.com/en-us/software-download/windows11) to create a bootable USB drive.
   > The `en-US x64` version is strongly recommended, as other versions may not function correctly. Additional language packs to change the display language can be installed later.
2. Place the `autounattend.xml` file in the root directory of the USB drive.
3. Boot the computer from the USB drive to begin the automated installation.

The installation process is almost entirely automatic; you only need to select the installation drive/partition.

## Optional Post-Installation Tools

### Language Pack

To install additional language packs, execute the following command in an administrative PowerShell console. For a list of available languages, refer to the [Microsoft documentation](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/available-language-packs-for-windows?view=windows-11#language-packs). The example below installs the Finnish language pack.

Note that the command may take over 15 minutes to complete, during which it might appear unresponsive.

```PowerShell
Install-Language -ExcludeFeatures:$true -CopyToSettings -Language fi-FI
```

### Windows Package Manager

For a unified way to install, manage, and keep your applications up-to-date, you can install [UniGetUI](https://github.com/marticliment/UniGetUI) by running the following commands in an administrative PowerShell console:

```PowerShell
Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force
Install-Script winget-install -Force
winget install --exact --id MartiCliment.UniGetUI --source winget --override "/NoAutoStart /ALLUSERS /VERYSILENT /SUPPRESSMSGBOXES /NORESTART"
```

#### Packages

For basic installations, I like to include the following packages. With the exception of the K-Lite Codec Pack (which is freeware), all are free open-source softwares.

**Essentials:**

- [ImageGlass](https://github.com/d2phap/ImageGlass) - Lightweight and versatile image viewer
- [K-Lite Codec Pack](https://codecguide.com/about_kl.htm) - Collection of codecs for playing a wide range of audio and video formats
- [LibreOffice](https://www.libreoffice.org/discover/libreoffice/) - Powerful and private office suite compatible with Microsoft Office
- [MPC-HC](https://github.com/clsid2/mpc-hc) - Lightweight and highly customizable media player
- [NanaZip](https://github.com/M2Team/NanaZip) - Modern file archiver based on 7-Zip

**Nice-to-haves:**

- [FFmpeg](https://github.com/BtbN/FFmpeg-Builds) - Comprehensive multimedia framework for handling audio and video
- [FreeTube](https://github.com/FreeTubeApp/FreeTube) - Private desktop YouTube player without ads or tracking
- [Greenshot](https://github.com/greenshot/greenshot) - Snipping tool on steroids with extensive annotation and export options
- [HandBrake](https://github.com/HandBrake/HandBrake) - Video transcoder for converting videos to various formats
- [Libre Hardware Monitor](https://github.com/LibreHardwareMonitor/LibreHardwareMonitor) - Tool for monitoring computer hardware, including temperatures, fan speeds, and voltages
- [LibreWolf](https://librewolf.net/) - Custom version of Firefox focused on privacy, security, and user freedom
- [MacType](https://github.com/snowie2000/mactype) - Utility for better font rendering in Windows (must have on OLED screens)
- [OBS Studio](https://github.com/obsproject/obs-studio) - Live streaming and screen recording
- [OpenRGB](https://gitlab.com/CalcProgrammer1/OpenRGB) - RGB lighting control for various components and peripherals
- [PowerToys](https://github.com/microsoft/PowerToys) - Collection of utilities for customizing Windows and streamlining tasks
- [Syncthing](https://github.com/syncthing/syncthing) - Continuous file synchronization between multiple computers/servers
- [VSCodium](https://github.com/VSCodium/vscodium) - Alternative to VS Code, without Microsoft's branding or telemetry
- [Windhawk](https://github.com/ramensoftware/windhawk) - Customization marketplace for Windows programs
- [WinDirStat](https://sourceforge.net/projects/windirstat/) - Disk usage statistics viewer and cleanup tool

## Key Features

This unattended configuration performs a wide range of modifications to streamline the user experience, enhance privacy, and remove unnecessary components.

### Installation & Account

- **Complete Hardware Check Bypass:** Skips the checks for TPM 2.0, Secure Boot, CPU, RAM, and storage, allowing installation on older or unsupported machines.
- **No Microsoft Account Required:** The setup is configured to bypass the mandatory online account creation.
- **Fully Automated OOBE:** Skips all setup prompts, including the End-User License Agreement (EULA) and network connection screens (BypassNRO).
- **Local Account by Default:** Creates a passwordless local administrator account named `user` and logs in automatically on the first boot.
- **No Password Expiration:** The local user's password is set to never expire.

### Privacy & Telemetry

- **Comprehensive Telemetry Disabled:** Disables system-level telemetry, data collection policies, feedback notifications, and the Customer Experience Improvement Program.
- **Activity & Data Collection Disabled:** Prevents tracking of app launches, user activities, and the advertising ID. Disables the location sensor and restricts inking/typing data collection.
- **Cloud Content Disabled:** Turns off "tailored experiences," cloud-optimized content, and consumer features.
- **PowerShell Telemetry Opt-Out:** Sets the system-wide environment variable to opt out of PowerShell data collection.

### User Interface & Experience

- **Classic Feel:** Restores the classic non-combined taskbar buttons and the classic right-click context menu.
- **Clean Desktop:** The Start Menu and Taskbar are left empty, providing a clean slate for customization. The "Recommended" section in the Start Menu is hidden.
- **Left-Aligned Taskbar:** The taskbar is aligned to the left, and icons for Chat, Task View, and Search are removed.
- **Dark Theme & Custom Appearance:** Sets the system and app theme to dark, enables transparency, and applies a new custom accent color (`#444759`) to the Start Menu, Taskbar, and window borders. A solid dark wallpaper (`#282A36`) is also set.
- **Sensible Defaults:** Configures File Explorer to open to "This PC," show hidden files, and display file extensions.
- **Reduced Animations & Sounds:** Disables most UI animations for a faster feel and turns off all system sounds.

### System & Performance

- **Stable Updates:** Defers Windows feature updates for 365 days and quality updates for 4 days, preventing you from being a beta tester. Crucial security updates are applied promptly.
- **Hibernation & Fast Startup Disabled:** Turns off hibernation and the Hiberboot feature for a more traditional shutdown process.
- **Performance Tweaks:** Disables filesystem last access time logging and prevents automatic device encryption (BitLocker).
- **Wake Timers Disabled:** Prevents devices and scheduled tasks from waking the computer.

### Removed Components & Bloatware

- **Core Components Removed:** Uninstalls OneDrive, Copilot, and disables the new Recall feature.
- **Extensive App Removal:** Automatically removes a large number of provisioned AppX packages and bloatware, with a safe list to preserve essential apps like Calculator, Notepad, and Paint.
- **Scheduled Tasks Disabled:** Removes unnecessary scheduled tasks related to application compatibility, data updates, and error reporting.

### Other Changes

Beyond the key features mentioned above, this configuration applies a large number of granular changes to the system for performance, privacy, and user experience. The following is a comprehensive list of those modifications:

- **Updates & Servicing:**
  - Sets Windows Update to automatically download and schedule installations, but disables automatic reboots if a user is logged in.
  - Disables the Delivery Optimization service, which prevents your PC from uploading updates to other PCs.
  - Removes the `C:\Windows.old` directory on the first logon to save space.
  - Removes the `unattend.xml` files from the system after setup is complete to ensure a clean state.
- **Services & Features:**
  - Disables the Remote Assistance feature.
  - Disables the telemetry uploader service (`dmwappushservice`).
  - Disables the location framework service.
  - Disables the Windows Ink Workspace.
  - Disables automatic map updates.
- **Network:**
  - Disables network access entirely until the first user logon is complete, then re-enables it.
  - Disables the system's active probing for internet connectivity status.
- **Input & Accessibility:**
  - Disables mouse acceleration for a more direct and predictable mouse response.
  - Disables the Sticky Keys accessibility prompt from appearing when the Shift key is pressed multiple times.
- **Miscellaneous:**
  - Prevents the automatic installation of Outlook and Dev Home.
  - Disables Automatic Restart Sign On (ARSO), which prevents Windows from automatically signing you in after an update.
  - Enables Auto Game Mode for better performance in games.
  - Disables audio ducking (the feature that lowers the volume of other applications when you are in a call).
- **Network Privacy:** Disables automatic connections to Wi-Fi Sense hotspots and the reporting of Wi-Fi hotspot data.
- **User Activity:**
  - Disables the Activity Feed feature.
  - Disables device-wide search history.
- **Input & Personalization:**
  - Prevents the preload of the Windows Input application.
  - Disables the Smart Clipboard feature.
  - Disables cloud-powered typing suggestions and online speech dictation for new users.
  - Resets suggested content and app recommendations for new users.
- **Security & Prompts:** Sets the UAC (User Account Control) to its default security level, prompting for consent on the secure desktop.
- **Microsoft Edge:**
  - Hides the first-run experience and welcome pages.
  - Disables the Hubs Sidebar.
  - Disables the startup boost and background running features.
- **Desktop & Taskbar:**
  - Hides most default icons (This PC, User's Files, Network, etc.) from the desktop, leaving only the Recycle Bin.
  - Enables the "End Task" option in the taskbar right-click context menu for new users.
  - Disables the News and Interests (Widgets) feature.
- **Start Menu & Lock Screen:**
  - Unlocks the Start Menu layout, allowing users to customize it after the initial setup.
  - Disables Windows Spotlight, tips, and slideshows on the lock screen for new users.
- **Visuals & Sound:**
  - Disables the Windows startup sound.
  - Increases the default cursor and text size for better visibility on HiDPI displays.

## Disclaimer

This project modifies the standard Windows installation process. Use it at your own risk. The author is not responsible for any data loss or damage that may occur.

## Acknowledgments

Thanks to the [unattend-generator](https://github.com/cschneegans/unattend-generator/) project, which provided the excellent baseline and generator.

## Unattend Generator URL

Note: This configuration includes manual adjustments that are not available through the web-based generator.

[Here is](https://schneegans.de/windows/unattend-generator/?LanguageMode=Unattended&UILanguage=en-US&Locale=fi-FI&Keyboard=0000040b&GeoLocation=77&ProcessorArchitecture=amd64&BypassRequirementsCheck=true&BypassNetworkCheck=true&ComputerNameMode=Random&CompactOsMode=Never&TimeZoneMode=Explicit&TimeZone=FLE+Standard+Time&PartitionMode=Interactive&DiskAssertionMode=Skip&WindowsEditionMode=Generic&WindowsEdition=pro&InstallFromMode=Index&InstallFromIndex=1&PEMode=Default&UserAccountMode=Unattended&AccountName0=user&AccountDisplayName0=&AccountPassword0=&AccountGroup0=Administrators&AutoLogonMode=Own&PasswordExpirationMode=Unlimited&LockoutMode=Default&HideFiles=HiddenSystem&ShowFileExtensions=true&ClassicContextMenu=true&LaunchToThisPC=true&ShowEndTask=true&TaskbarSearch=Hide&TaskbarIconsMode=Empty&DisableWidgets=true&LeftTaskbar=true&HideTaskViewButton=true&DisableBingResults=true&StartTilesMode=Empty&StartPinsMode=Empty&DisableFastStartup=true&DisableLastAccess=true&TurnOffSystemSounds=true&DisableAppSuggestions=true&PreventDeviceEncryption=true&HideEdgeFre=true&DisableEdgeStartupBoost=true&DisablePointerPrecision=true&DeleteWindowsOld=true&EffectsMode=Custom&DWMAeroPeekEnabled=true&ListviewShadow=true&ThumbnailsOrIcon=true&ListviewAlphaSelect=true&DragFullWindows=true&FontSmoothing=true&ListBoxSmoothScrolling=true&DropShadow=true&DesktopIconsMode=Custom&IconRecycleBin=true&IconThisPC=true&StartFoldersMode=Default&WifiMode=Skip&ExpressSettings=DisableAll&LockKeysMode=Skip&StickyKeysMode=Disabled&ColorMode=Custom&SystemColorTheme=Dark&AppsColorTheme=Dark&AccentColor=%23444759&AccentColorOnStart=true&AccentColorOnBorders=true&EnableTransparency=true&WallpaperMode=Solid&WallpaperColor=%23282a36&LockScreenMode=Default&SystemScript0=%23+Remove+scheduled+task%3A+Application+Compatibility+Appraiser%0D%0Aschtasks+%2FDelete+%2FTN+%22%5CMicrosoft%5CWindows%5CApplication+Experience%5CMicrosoft+Compatibility+Appraiser%22+%2FF%0D%0A%0D%0A%23+Remove+scheduled+task%3A+Program+Data+Updater%0D%0Aschtasks+%2FDelete+%2FTN+%22%5CMicrosoft%5CWindows%5CApplication+Experience%5CProgramDataUpdater%22+%2FF%0D%0A%0D%0A%23+Remove+scheduled+task%3A+Chkdsk+Proxy%0D%0Aschtasks+%2FDelete+%2FTN+%22%5CMicrosoft%5CWindows%5CChkdsk%5CProxy%22+%2FF%0D%0A%0D%0A%23+Remove+scheduled+task%3A+Windows+Error+Reporting%0D%0Aschtasks+%2FDelete+%2FTN+%22%5CMicrosoft%5CWindows%5CWindows+Error+Reporting%5CQueueReporting%22+%2FF%0D%0A%0D%0A%23+Remove+scheduled+task%3A+Customer+Experience+Improvement+Program%0D%0A%24ceipPath+%3D+%22%24env%3ASystemRoot%5CSystem32%5CTasks%5CMicrosoft%5CWindows%5CCustomer+Experience+Improvement+Program%22%0D%0Aif+%28Test-Path+%24ceipPath%29+%7B%0D%0A++Get-ChildItem+%24ceipPath+%7C+ForEach-Object+%7B%0D%0A++++%24taskName+%3D+%24_.Name%0D%0A++++schtasks+%2FDelete+%2FTN+%22%5CMicrosoft%5CWindows%5CCustomer+Experience+Improvement+Program%5C%24taskName%22+%2FF%0D%0A++%7D%0D%0A%7D%0D%0A%0D%0A%23+Remove+Copilot%0D%0ADISM+%2FOnline+%2FRemove-Package+%2FPackage-Name%3AMicrosoft.Windows.Copilot%0D%0A%0D%0A%23+Remove+Recall%0D%0ADISM+%2FOnline+%2FDisable-Feature+%2FFeatureName%3ARecall%0D%0A%0D%0A%23+Disable+network+access+until+first+logon%0D%0AGet-NetAdapter+%7C+Disable-NetAdapter+-Confirm%3A%24false%0D%0A%0D%0A%23+Prevent+automatic+installation+of+Outlook+and+Dev+Home%0D%0ARemove-Item+-Path+%22HKLM%3A%5CSoftware%5CMicrosoft%5CWindowsUpdate%5COrchestrator%5CUScheduler_Oobe%5CDevHomeUpdate%22+-Force+-ErrorAction+SilentlyContinue%0D%0ARemove-Item+-Path+%22HKLM%3A%5CSoftware%5CMicrosoft%5CWindowsUpdate%5COrchestrator%5CUScheduler_Oobe%5COutlookUpdate%22+-Force+-ErrorAction+SilentlyContinue%0D%0A%0D%0A%23+Remove+OneDrive%0D%0AStop-Process+-Force+-Name+OneDrive+-ErrorAction+SilentlyContinue+%7C+Out-Null%0D%0ARemove-Item+-LiteralPath+%27C%3A%5CUsers%5CDefault%5CAppData%5CRoaming%5CMicrosoft%5CWindows%5CStart+Menu%5CPrograms%5COneDrive.lnk%27%2C+%27C%3A%5CUsers%5CDefault%5CAppData%5CRoaming%5CMicrosoft%5CWindows%5CStart+Menu%5CPrograms%5COneDrive.exe%27%2C+%27C%3A%5CWindows%5CSystem32%5COneDriveSetup.exe%27%2C+%27C%3A%5CWindows%5CSysWOW64%5COneDriveSetup.exe%27+-ErrorAction+%27Continue%27%0D%0A%0D%0A%23%23%23+REMOVE+APPS%2C+CAPABILITIES+AND+OPTIONAL+FEATURES+%23%23%23%0D%0A%0D%0A%23+Explicitly+excluded+package+name+patterns%0D%0A%24excludedPatterns+%3D+%40%28%0D%0A++%22AppUp.ThunderboltControlCenter%22%2C%0D%0A++%22Microsoft-RemoteDesktopConnection%22%2C%0D%0A++%22Microsoft-Windows-Subsystem-Linux%22%2C%0D%0A++%22Microsoft.GamingApp%22%2C%0D%0A++%22Microsoft.OpenSSHClient%22%2C%0D%0A++%22Microsoft.Paint%22%2C%0D%0A++%22Microsoft.Windows.Notepad%22%2C%0D%0A++%22Microsoft.WindowsCalculator%22%2C%0D%0A++%22Microsoft.WindowsNotepad%22%2C%0D%0A++%22Microsoft.Xbox%22%2C%0D%0A++%22NVIDIACorp.NVIDIAControlPanel%22%2C%0D%0A++%22OpenSSH.Client%22%2C%0D%0A++%22Printing-PrintToPDFServices-Features%22%2C%0D%0A++%22StorePurchaseApp%22%2C%0D%0A++%22VirtualMachinePlatform%22%0D%0A%29+-join+%27%7C%27%0D%0A%0D%0A%23+Unsafe%2Funwanted+to+remove%0D%0A%24unsafePatterns+%3D+%28%0D%0A++%27ApplicationCompatibility%7CEthernet.Client%7CImageExtension%7CLanguage.Basic%7CMediaExtension%7C%27+%2B%0D%0A++%27NetFX%7CPowerShell%7CPrint.Management%7CPrinting-Foundation-Features%7CSearchEngine-Client-Package%7C%27+%2B%0D%0A++%27VBScript%7CVideoExtension%7CWMIC%7CWifi.Client%7CWinAppRuntime%7CWindowsStore%7CWindowsTerminal%27%0D%0A%29%0D%0A%0D%0A%23+Cache+provisioned+packages+and+names%0D%0A%24provisionedPackages+%3D+Get-AppxProvisionedPackage+-Online%0D%0A%24provisionedNames+%3D+%24provisionedPackages+%7C+Select-Object+-ExpandProperty+DisplayName+-Unique%0D%0A%0D%0A%23+Filtered+package+list%0D%0A%24targetPackages+%3D+Get-AppxPackage+-AllUsers+%7C+Where-Object+%7B%0D%0A++%28-not+%24_.NonRemovable%29+-and%0D%0A++%28-not+%24_.IsFramework%29+-and%0D%0A++%28%24provisionedNames+-contains+%24_.Name%29+-and%0D%0A++%28%24_.Name+-notmatch+%24unsafePatterns%29+-and%0D%0A++%28%24_.Name+-notmatch+%24excludedPatterns%29%0D%0A%7D%0D%0A%0D%0A%23+Filtered+Windows+Capability+list%0D%0A%24targetCapabilities+%3D+Get-WindowsCapability+-Online+%7C+Where-Object+%7B%0D%0A++%28%24_.State+-notin+%40%28%27NotPresent%27%2C+%27Removed%27%29%29+-and%0D%0A++%28%24_.Name+-notmatch+%24unsafePatterns%29+-and%0D%0A++%28%24_.Name+-notmatch+%24excludedPatterns%29%0D%0A%7D%0D%0A%0D%0A%23+Filtered+Windows+Optional+Features+list%0D%0A%24targetFeatures+%3D+Get-WindowsOptionalFeature+-Online+%7C+Where-Object+%7B%0D%0A++%28%24_.State+-notin+%40%28%27Disabled%27%2C+%27DisabledWithPayloadRemoved%27%29%29+-and%0D%0A++%28%24_.FeatureName+-notmatch+%24unsafePatterns%29+-and%0D%0A++%28%24_.FeatureName+-notmatch+%24excludedPatterns%29%0D%0A%7D%0D%0A%0D%0A%23+Check+if+non-system+users+exist%0D%0A%24usersExist+%3D+Get-LocalUser+%7C+Where-Object+%7B%0D%0A++%28%24_.Enabled+-eq+%24true%29+-and%0D%0A++%28%24_.Name+-notin+%27Administrator%27%2C+%27DefaultAccount%27%2C+%27Guest%27%2C+%27WDAGUtilityAccount%27%29%0D%0A%7D%0D%0A%0D%0A%23+If+users+exist%2C+remove+packages+from+all+current+users%0D%0Aif+%28%24usersExist.Count+-gt+0%29+%7B%0D%0A++%24targetPackages+%7C+Remove-AppxPackage+-AllUsers+-ErrorAction+SilentlyContinue%0D%0A%7D%0D%0A%0D%0A%23+Remove+provisioned+packages%0D%0A%24targetPackagesNames+%3D+%24targetPackages+%7C+Select-Object+-ExpandProperty+Name+-Unique%0D%0A%24provisionedPackages+%7C+Where-Object+%7B+%24targetPackagesNames+-contains+%24_.DisplayName+%7D+%7C%0D%0A++Remove-AppxProvisionedPackage+-Online+-AllUsers+-ErrorAction+SilentlyContinue%0D%0A%0D%0A%23+Remove+Windows+Capabilities%0D%0A%24targetCapabilities+%7C+Remove-WindowsCapability+-Online+-ErrorAction+SilentlyContinue%0D%0A%0D%0A%23+Disable+Windows+Optional+Features%0D%0A%24targetFeatures+%7C+Disable-WindowsOptionalFeature+-Online+-Remove+-NoRestart+-ErrorAction+SilentlyContinue&SystemScriptType0=Ps1&SystemScript1=Windows+Registry+Editor+Version+5.00%0D%0A%0D%0A%3B+Hack+to+get+clean+Start+Menu%0D%0A%5BHKEY_LOCAL_MACHINE%5CSOFTWARE%5CMicrosoft%5CPolicyManager%5Ccurrent%5Cdevice%5CEducation%5D%0D%0A%22IsEducationEnvironment%22%3Ddword%3A00000001%0D%0A%0D%0A%3B+Hide+%27Recommended%27+section+in+Start+menu%0D%0A%5BHKEY_LOCAL_MACHINE%5CSOFTWARE%5CMicrosoft%5CPolicyManager%5Ccurrent%5Cdevice%5CStart%5D%0D%0A%22HideRecommendedSection%22%3Ddword%3A00000001%0D%0A%0D%0A%3B+Disable+automatic+connection+to+WiFi+Sense+hotspots%0D%0A%5BHKEY_LOCAL_MACHINE%5CSOFTWARE%5CMicrosoft%5CPolicyManager%5Cdefault%5CWiFi%5CAllowAutoConnectToWiFiSenseHotspots%5D%0D%0A%22Value%22%3Ddword%3A00000000%0D%0A%0D%0A%3B+Do+not+send+analytics+about+WiFi+hotspots%0D%0A%5BHKEY_LOCAL_MACHINE%5CSOFTWARE%5CMicrosoft%5CPolicyManager%5Cdefault%5CWiFi%5CAllowWiFiHotSpotReporting%5D%0D%0A%22Value%22%3Ddword%3A00000000%0D%0A%0D%0A%3B+Set+location+sensor+permission+denied%0D%0A%5BHKEY_LOCAL_MACHINE%5CSOFTWARE%5CMicrosoft%5CWindows+NT%5CCurrentVersion%5CSensor%5COverrides%5C%7BBFA794E4-F964-4FDB-90F6-51056BFE4B44%7D%5D%0D%0A%22SensorPermissionState%22%3Ddword%3A00000000%0D%0A%0D%0A%3B+Deny+all+apps+system-wide+access+to+location%0D%0A%5BHKEY_LOCAL_MACHINE%5CSOFTWARE%5CMicrosoft%5CWindows%5CCurrentVersion%5CCapabilityAccessManager%5CConsentStore%5Clocation%5D%0D%0A%22Value%22%3D%22Deny%22%0D%0A%0D%0A%3B+Prevent+automatic+install+of+Chat+app%0D%0A%5BHKEY_LOCAL_MACHINE%5CSOFTWARE%5CMicrosoft%5CWindows%5CCurrentVersion%5CCommunications%5D%0D%0A%22ConfigureChatAutoInstall%22%3Ddword%3A00000000%0D%0A%0D%0A%3B+Disable+telemetry+at+Windows+system+level%0D%0A%5BHKEY_LOCAL_MACHINE%5CSOFTWARE%5CMicrosoft%5CWindows%5CCurrentVersion%5CPolicies%5CDataCollection%5D%0D%0A%22AllowTelemetry%22%3Ddword%3A00000000%0D%0A%0D%0A%3B+Disable+grouping+similar+taskbar+buttons%0D%0A%5BHKEY_LOCAL_MACHINE%5CSOFTWARE%5CMicrosoft%5CWindows%5CCurrentVersion%5CPolicies%5CExplorer%5D%0D%0A%22NoTaskGrouping%22%3Ddword%3A00000001%0D%0A%0D%0A%3B+Disable+Automatic+Restart+Sign+On+%28ARSO%29%0D%0A%3B+Prompt+consent+on+the+secure+desktop+for+UAC%0D%0A%3B+Prompt+for+consent%0D%0A%5BHKEY_LOCAL_MACHINE%5CSOFTWARE%5CMicrosoft%5CWindows%5CCurrentVersion%5CPolicies%5CSystem%5D%0D%0A%22DisableAutomaticRestartSignOn%22%3Ddword%3A00000001%0D%0A%22PromptOnSecureDesktop%22%3Ddword%3A00000001%0D%0A%22EnableLUA%22%3Ddword%3A00000001%0D%0A%22ConsentPromptBehaviorAdmin%22%3Ddword%3A00000005%0D%0A%0D%0A%3B+Defer+feature+updates+for+365+days+and+quality+updates+for+4+days%0D%0A%5BHKEY_LOCAL_MACHINE%5CSOFTWARE%5CMicrosoft%5CWindowsUpdate%5CUX%5CSettings%5D%0D%0A%22DeferFeatureUpdatesPeriodInDays%22%3Ddword%3A0000016d%0D%0A%22DeferQualityUpdatesPeriodInDays%22%3Ddword%3A00000004%0D%0A%0D%0A%3B+Disables+Microsoft+Edge%27s+Hubs+Sidebar%0D%0A%5BHKEY_LOCAL_MACHINE%5CSOFTWARE%5CPolicies%5CMicrosoft%5CEdge%5D%0D%0A%22HubsSidebarEnabled%22%3Ddword%3A00000000%0D%0A%0D%0A%3B+Disable+push+to+install%0D%0A%5BHKEY_LOCAL_MACHINE%5CSOFTWARE%5CPolicies%5CMicrosoft%5CPushToInstall%5D%0D%0A%22DisablePushToInstall%22%3Ddword%3A00000001%0D%0A%0D%0A%3B+Disable+the+Advertising+ID+feature+for+apps%0D%0A%5BHKEY_LOCAL_MACHINE%5CSOFTWARE%5CPolicies%5CMicrosoft%5CWindows%5CAdvertisingInfo%5D%0D%0A%22DisabledByGroupPolicy%22%3Ddword%3A00000001%0D%0A%0D%0A%3B+Disable+automatic+archiving+of+apps+not+used+frequently%0D%0A%5BHKEY_LOCAL_MACHINE%5CSOFTWARE%5CPolicies%5CMicrosoft%5CWindows%5CAppx%5D%0D%0A%22AllowAutomaticAppArchiving%22%3Ddword%3A00000000%0D%0A%0D%0A%3B+Disables+content+and+suggestions+based+on+the+signed-in+consumer+account%0D%0A%5BHKEY_LOCAL_MACHINE%5CSOFTWARE%5CPolicies%5CMicrosoft%5CWindows%5CCloudContent%5D%0D%0A%22DisableConsumerAccountStateContent%22%3Ddword%3A00000001%0D%0A%0D%0A%3B+Disable+telemetry%0D%0A%3B+Disable+feedback+notifications%0D%0A%5BHKEY_LOCAL_MACHINE%5CSOFTWARE%5CPolicies%5CMicrosoft%5CWindows%5CDataCollection%5D%0D%0A%22AllowTelemetry%22%3Ddword%3A00000000%0D%0A%22DoNotShowFeedbackNotifications%22%3Ddword%3A00000001%0D%0A%0D%0A%3B+Disable+Delivery+Optimization%0D%0A%5BHKEY_LOCAL_MACHINE%5CSOFTWARE%5CPolicies%5CMicrosoft%5CWindows%5CDeliveryOptimization%5D%0D%0A%22DODownloadMode%22%3Ddword%3A00000000%0D%0A%0D%0A%3B+Disable+app+launch+tracking%0D%0A%5BHKEY_LOCAL_MACHINE%5CSOFTWARE%5CPolicies%5CMicrosoft%5CWindows%5CEdgeUI%5D%0D%0A%22DisableMFUTracking%22%3Ddword%3A00000001%0D%0A%0D%0A%3B+Do+not+automatically+encrypt+eDrives%0D%0A%5BHKEY_LOCAL_MACHINE%5CSOFTWARE%5CPolicies%5CMicrosoft%5CWindows%5CEnhancedStorageDevices%5D%0D%0A%22TCGSecurityActivationDisabled%22%3Ddword%3A00000001%0D%0A%0D%0A%3B+Hide+%27Recommended%27+section+in+Start+Menu%0D%0A%5BHKEY_LOCAL_MACHINE%5CSOFTWARE%5CPolicies%5CMicrosoft%5CWindows%5CExplorer%5D%0D%0A%22HideRecommendedSection%22%3Ddword%3A00000001%0D%0A%0D%0A%3B+Disable+Storage+Sense+system-wide%0D%0A%5BHKEY_LOCAL_MACHINE%5CSOFTWARE%5CPolicies%5CMicrosoft%5CWindows%5CStorageSense%5D%0D%0A%22AllowStorageSenseGlobal%22%3Ddword%3A00000000%0D%0A%0D%0A%3B+Disable+lock+screen+app+notifications%0D%0A%3B+Disable+Activity+Feed+feature%0D%0A%3B+Prevent+publishing%2Fuploading+user+activities+to+Microsoft+cloud%0D%0A%5BHKEY_LOCAL_MACHINE%5CSOFTWARE%5CPolicies%5CMicrosoft%5CWindows%5CSystem%5D%0D%0A%22DisableLockScreenAppNotifications%22%3Ddword%3A00000001%0D%0A%22EnableActivityFeed%22%3Ddword%3A00000000%0D%0A%22PublishUserActivities%22%3Ddword%3A00000000%0D%0A%22UploadUserActivities%22%3Ddword%3A00000000%0D%0A%0D%0A%3B+Disable+Windows+Error+Reporting%0D%0A%5BHKEY_LOCAL_MACHINE%5CSOFTWARE%5CPolicies%5CMicrosoft%5CWindows%5CWindows+Error+Reporting%5D%0D%0A%22Disabled%22%3Ddword%3A00000001%0D%0A%0D%0A%3B+Automatically+download+and+scheduled+installation+of+Windows+Update%0D%0A%3B+Disable+auto-reboot+with+users+logged+in%0D%0A%5BHKEY_LOCAL_MACHINE%5CSOFTWARE%5CPolicies%5CMicrosoft%5CWindows%5CWindowsUpdate%5CAU%5D%0D%0A%22AUOptions%22%3Ddword%3A00000004%0D%0A%22NoAutoRebootWithLoggedOnUsers%22%3Ddword%3A00000001%0D%0A%0D%0A%3B+Disable+Windows+Ink+Workspace+feature%0D%0A%5BHKEY_LOCAL_MACHINE%5CSOFTWARE%5CPolicies%5CMicrosoft%5CWindowsInkWorkspace%5D%0D%0A%22AllowWindowsInkWorkspace%22%3Ddword%3A00000000%0D%0A%0D%0A%3B+Disable+hibernation%0D%0A%5BHKEY_LOCAL_MACHINE%5CSYSTEM%5CCurrentControlSet%5CControl%5CPower%5D%0D%0A%22HibernateEnabled%22%3Ddword%3A00000000%0D%0A%22HibernateEnabledDefault%22%3Ddword%3A00000000%0D%0A%0D%0A%3B+Disable+Remote+Assistance%0D%0A%5BHKEY_LOCAL_MACHINE%5CSYSTEM%5CCurrentControlSet%5CControl%5CRemote+Assistance%5D%0D%0A%22fAllowToGetHelp%22%3Ddword%3A00000000%0D%0A%0D%0A%3B+Disables+telemetry+uploader%0D%0A%5BHKEY_LOCAL_MACHINE%5CSYSTEM%5CCurrentControlSet%5CServices%5Cdmwappushservice%5D%0D%0A%22Start%22%3Ddword%3A00000004%0D%0A%0D%0A%3B+Disable+location+framework+service%0D%0A%5BHKEY_LOCAL_MACHINE%5CSYSTEM%5CCurrentControlSet%5CServices%5Clfsvc%5CService%5CConfiguration%5D%0D%0A%22Status%22%3Ddword%3A00000000%0D%0A%0D%0A%3B+Disable+probing+for+network+connectivity+status%0D%0A%5BHKEY_LOCAL_MACHINE%5CSYSTEM%5CCurrentControlSet%5CServices%5CNlaSvc%5CParameters%5CInternet%5D%0D%0A%22EnableActiveProbing%22%3Ddword%3A00000000%0D%0A%0D%0A%3B+Disable+map+updates%0D%0A%5BHKEY_LOCAL_MACHINE%5CSYSTEM%5CMaps%5D%0D%0A%22AutoUpdateEnabled%22%3Ddword%3A00000000&SystemScriptType1=Reg&DefaultUserScript0=%23+Removes+the+OneDriveSetup+auto-start+entry+for+the+Default+User%0D%0ARemove-ItemProperty+-LiteralPath+%27Registry%3A%3AHKU%5CDefaultUser%5CSoftware%5CMicrosoft%5CWindows%5CCurrentVersion%5CRun%27+-Name+%27OneDriveSetup%27+-Force+-ErrorAction+%27Continue%27%0D%0A%0D%0A%23+Resets+suggested+content+and+app+recommendations%0D%0ARemove-ItemProperty+-LiteralPath+%27Registry%3A%3AHKU%5CDefaultUser%5CSoftware%5CMicrosoft%5CWindows%5CCurrentVersion%5CContentDeliveryManager%27+-Name+%27Subscriptions%27+-Force+-ErrorAction+%27Continue%27%0D%0ARemove-ItemProperty+-LiteralPath+%27Registry%3A%3AHKU%5CDefaultUser%5CSoftware%5CMicrosoft%5CWindows%5CCurrentVersion%5CContentDeliveryManager%27+-Name+%27SuggestedApps%27+-Force+-ErrorAction+%27Continue%27&DefaultUserScriptType0=Ps1&DefaultUserScript1=Windows+Registry+Editor+Version+5.00%0D%0A%0D%0A%3B+Disable+Windows+Spotlight+Lock+Screen+images%0D%0A%3B+Disable+tips%2C+fun+facts%2C+and+more+on+the+lock+screen%0D%0A%3B+Disable+slideshow+as+lock+screen+background%0D%0A%3B+Disable+%27Suggested+Apps%27+and+%27Suggestions%27%0D%0A%5BHKEY_USERS%5CDefaultUser%5CSoftware%5CMicrosoft%5CWindows%5CCurrentVersion%5CContentDeliveryManager%5D%0D%0A%22RotatingLockScreenEnabled%22%3Ddword%3A00000000%0D%0A%22RotatingLockScreenOverlayEnabled%22%3Ddword%3A00000000%0D%0A%22SlideshowEnabled%22%3Ddword%3A00000000%0D%0A%22SubscribedContent-314563Enabled%22%3Ddword%3A00000000%0D%0A%22SubscribedContent-353694Enabled%22%3Ddword%3A00000000%0D%0A%22SubscribedContent-353696Enabled%22%3Ddword%3A00000000%0D%0A%0D%0A%3B+Disable+Game+DVR+and+game+recording%0D%0A%5BHKEY_USERS%5CDefaultUser%5CSoftware%5CMicrosoft%5CWindows%5CCurrentVersion%5CGameDVR%5D%0D%0A%22AppCaptureEnabled%22%3Ddword%3A00000000%0D%0A%0D%0A%3B+Disables+%22tailored+experiences%22%0D%0A%5BHKEY_USERS%5CDefaultUser%5CSoftware%5CMicrosoft%5CWindows%5CCurrentVersion%5CPrivacy%5D%0D%0A%22TailoredExperiencesWithDiagnosticDataEnabled%22%3Ddword%3A00000000%0D%0A%0D%0A%3B+Disables+cloud-powered+typing+suggestions%0D%0A%5BHKEY_USERS%5CDefaultUser%5CSoftware%5CMicrosoft%5CInput%5CTIPC%5D%0D%0A%22Enabled%22%3Ddword%3A00000000%0D%0A%0D%0A%3B+Disables+online+speech+dictation%0D%0A%5BHKEY_USERS%5CDefaultUser%5CSoftware%5CMicrosoft%5CSpeech_OneCore%5CSettings%5COnlineSpeechPrivacy%5D%0D%0A%22HasAccepted%22%3Ddword%3A00000000%0D%0A%0D%0A%3B+Disable+Windows+Copilot+sidebar%0D%0A%5BHKEY_USERS%5CDefaultUser%5CSoftware%5CPolicies%5CMicrosoft%5CWindows%5CWindowsCopilot%5D%0D%0A%22TurnOffWindowsCopilot%22%3Ddword%3A00000001&DefaultUserScriptType1=Reg&FirstLogonScript0=%23+Enable+previously+disabled+network+adapter%0D%0AGet-NetAdapter+%7C+Enable-NetAdapter+-Confirm%3A%24false%0D%0A%0D%0A%23+Disable+PowerShell+telemetry%0D%0A%5BEnvironment%5D%3A%3ASetEnvironmentVariable%28%27POWERSHELL_TELEMETRY_OPTOUT%27%2C+%271%27%2C+%27Machine%27%29%0D%0A%0D%0A%23+Get+wake-armed+devices%0D%0A%24devices+%3D+Powercfg+%2FDEVICEQUERY+Wake_Armed+%7C+Where-Object+%7B+%24_.Trim%28%29+-ne+%22%22+-and+%24_.Trim%28%29+-ne+%22NONE%22+%7D%0D%0A%0D%0A%23+Disable+device+wake%0D%0Aif+%28%24devices%29+%7B%0D%0A++++ForEach+%28%24dev+in+%24devices%29+%7B%0D%0A++++++++Powercfg+%2FDEVICEENABLEWAKE+%22%24dev%22%0D%0A++++%7D%0D%0A%7D%0D%0A%0D%0A%23+Disable+wake+timers%0D%0APowercfg+%2FSETACVALUEINDEX+SCHEME_CURRENT+238c9fa8-0aad-41ed-83f4-97be242c8f20+bd3b718a-0680-4d9d-8ab2-e1d2b4ac806d+0%0D%0APowercfg+%2FSETDCVALUEINDEX+SCHEME_CURRENT+238c9fa8-0aad-41ed-83f4-97be242c8f20+bd3b718a-0680-4d9d-8ab2-e1d2b4ac806d+0%0D%0APowercfg+%2FSETACVALUEINDEX+SCHEME_CURRENT+SUB_SLEEP+RTCWAKE+0%0D%0APowercfg+%2FSETDCVALUEINDEX+SCHEME_CURRENT+SUB_SLEEP+RTCWAKE+0&FirstLogonScriptType0=Ps1&UserOnceScript0=Windows+Registry+Editor+Version+5.00%0D%0A%0D%0A%3B+Increase+cursor+size+and+text+for+HiDPI+displays%0D%0A%5BHKEY_CURRENT_USER%5CSoftware%5CMicrosoft%5CAccessibility%5D%0D%0A%22CursorSize%22%3Ddword%3A00000003%0D%0A%22CursorType%22%3Ddword%3A00000003%0D%0A%22TextScaleFactor%22%3Ddword%3A00000070%0D%0A%0D%0A%3B+Enable+Auto+Game+Mode+for+better+performance+in+games%0D%0A%5BHKEY_CURRENT_USER%5CSoftware%5CMicrosoft%5CGameBar%5D%0D%0A%22AutoGameModeEnabled%22%3Ddword%3A00000001%0D%0A%0D%0A%3B+Prevent+preload+of+the+Windows+Input+app%0D%0A%5BHKEY_CURRENT_USER%5CSoftware%5CMicrosoft%5Cinput%5D%0D%0A%22IsInputAppPreloadEnabled%22%3Ddword%3A00000000%0D%0A%0D%0A%3B+Restrict+inking+data+and+text+collection%0D%0A%5BHKEY_CURRENT_USER%5CSoftware%5CMicrosoft%5CInputPersonalization%5D%0D%0A%22RestrictImplicitInkCollection%22%3Ddword%3A00000001%0D%0A%22RestrictImplicitTextCollection%22%3Ddword%3A00000001%0D%0A%0D%0A%3B+Prevents+inking+and+typing+data+from+being+used+to+improve+recognition%0D%0A%5BHKEY_CURRENT_USER%5CSoftware%5CMicrosoft%5CInputPersonalization%5CTrainedDataStore%5D%0D%0A%22HarvestContacts%22%3Ddword%3A00000000%0D%0A%0D%0A%3B+No+audio+ducking%0D%0A%5BHKEY_CURRENT_USER%5CSoftware%5CMicrosoft%5CMultimedia%5CAudio%5D%0D%0A%22UserDuckingPreference%22%3Ddword%3A00000003%0D%0A%0D%0A%3B+Do+not+accept+privacy+policy%0D%0A%5BHKEY_CURRENT_USER%5CSoftware%5CMicrosoft%5CPersonalization%5CSettings%5D%0D%0A%22AcceptedPrivacyPolicy%22%3Ddword%3A00000000%0D%0A%0D%0A%3B+Disable+inking+and+typing+personalization+data+collection%0D%0A%5BHKEY_CURRENT_USER%5CSoftware%5CMicrosoft%5CWindows%5CCurrentVersion%5CCPSS%5CStore%5CInkingAndTypingPersonalization%5D%0D%0A%22Value%22%3Ddword%3A00000000%0D%0A%0D%0A%3B+Prevent+pre-launch+of+Start+Menu+related+background+process%0D%0A%5BHKEY_CURRENT_USER%5CSoftware%5CMicrosoft%5CWindows%5CCurrentVersion%5CDsh%5D%0D%0A%22IsPrelaunchEnabled%22%3Ddword%3A00000000%0D%0A%0D%0A%3B+Do+not+track+frequently+used+programs+in+Start+Menu%0D%0A%5BHKEY_CURRENT_USER%5CSOFTWARE%5CMicrosoft%5CWindows%5CCurrentVersion%5CExplorer%5CAdvanced%5D%0D%0A%22Start_TrackProgs%22%3Ddword%3A00000000%0D%0A%0D%0A%3B+Disable+Game+DVR%0D%0A%5BHKEY_CURRENT_USER%5CSystem%5CGameConfigStore%5D%0D%0A%22GameDVR_Enabled%22%3Ddword%3A00000000%0D%0A%0D%0A%3B+Do+not+group+taskbar+buttons+for+same+program+%28terrible+implementation%29%0D%0A%5BHKEY_CURRENT_USER%5CSoftware%5CMicrosoft%5CWindows%5CCurrentVersion%5CPolicies%5CExplorer%5D%0D%0A%22NoTaskGrouping%22%3Ddword%3A00000001%0D%0A%0D%0A%3B+Disable+device-wide+search+history%0D%0A%5BHKEY_CURRENT_USER%5CSoftware%5CMicrosoft%5CWindows%5CCurrentVersion%5CSearchSettings%5D%0D%0A%22IsDeviceSearchHistoryEnabled%22%3Ddword%3A00000000%0D%0A%0D%0A%3B+Disable+Smart+Clipboard+feature%0D%0A%5BHKEY_CURRENT_USER%5CSoftware%5CMicrosoft%5CWindows%5CCurrentVersion%5CSmartActionPlatform%5CSmartClipboard%5D%0D%0A%22Disabled%22%3Ddword%3A00000001%0D%0A%0D%0A%3B+Disable+app+launch+tracking%0D%0A%5BHKEY_CURRENT_USER%5CSoftware%5CPolicies%5CMicrosoft%5CWindows%5CEdgeUI%5D%0D%0A%22DisableMFUTracking%22%3Ddword%3A00000001%0D%0A%0D%0A%3B+Disable+in-game+recording%0D%0A%5BHKEY_CURRENT_USER%5CSoftware%5CMicrosoft%5CWindows%5CCurrentVersion%5CGameDVR%5D%0D%0A%22AppCaptureEnabled%22%3Ddword%3A00000000&UserOnceScriptType0=Reg&RestartExplorer=true&WdacMode=Skip) the world longest URL to the generator.
