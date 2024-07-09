<!-- README.md -->

# VMInfo.dat Summary

The **VMInfo.dat** file is a log and configuration file for managing virtual machines (VMs). It stores various details about the VM, including installed packages, system states, configurations, and any errors that occur. Here’s what each section represents:

# Host and File Information
- Put any HOST information that you want to keep, just in case you need it.

# VM Information
  - OS: Operating system running on the VM (e.g., FreeBSD 14.1 Release).
  - Type: Type of OS (e.g., BSD UNIX).
  - Architecture: System architecture (e.g., X86_64, ARM, i368, PPC, ETC.).
  - Installed Dependencies: List of packages installed by the user, categorized by necessity and purpose, Listed on **DATE_ADDED**.

# Disk Information
   - Max Size Drive: Maximum disk size allocated to the VM.(e.g., MAX_DISK_SIZE=64GiB ## Maximum Disk Size the VM can use, is 64GB / NOTE:"GiB" is ANOTHER file meassurement, but i used it so I could understand what the file reads BETTER, change the size format to fit your necesities.)
   - Drive Format: Format of the disk and boot method.(e.g., VHD, VDA, QCOW2. IMG, ISO, ETC.)

# Userland Configuration
   - Non-Root User: Non-root user account details (username, password).
   - Root User: Root account details (password, SSH access settings).

# VM State and Configuration
   - Errors: Records any errors encountered.
   - XORG Configuration: Details about XORG (display server) setup.
   - DateTime Control: Synchronization status of the system clock.
   - Internet Link: Internet connectivity status.
   - Kernel Panic State: Whether the kernel is in a panic state.

# Date Log
   - Date: Logs the date for tracking VM usage.

     **Final State**
        - VM Boot Status: Indicates if the VM can boot successfully.
        - Kernel Integrity: Health status of the kernel.
        - Boot Counts: Number of times the VM has been booted successfully.
        - Installer State: Status of the installer ISO (whether it's still needed).

## Purpose

The **VMInfo.dat** file is designed to keep track of the VM's configuration and state, making it slightly-easier to monitor, troubleshoot, and manage the VM. It helps you log important details, such as installed packages, system configurations, and any errors, providing a slight-comprehensve overview of the VM's health and activity history.
📝 NOTE: there's better options and i highly encourage to use them, but if none of them work, you're free to use this.



# SYNTAX EXAMPLES 📝 :

## Set Section `<!-- VMINFO --!>`
- Set a OS Name for your VM.
    - `OS_NAME="Your VM's OS Name Here Followed By It's Version."`

      Example:

   ```
   OS="FreeBSD V14.1 Release"
   ```

- Define a OS_TYPE for your VM
  - `TYPE="OS_NAME_HERE OS_FAMILY e.g, BSD UNIX / LINUX UNIX / WINDOWS NT / ETC FAMILY_ETC"`

    Example:

    ```
    TYPE="BSD UNIX"
    ```

- Define VM_ARCHITECTURE.
 | This is very important, since if an architecture is not available, you might find issues when using a *DISK_RECOVERY* utility to flash the VM's VHD/QCOW2/IMG to a real *STORAGE_DEVICE* for use on REAL_HARDWARE

 - `ARCH="X86_64, ARM, PPC, ETC."`

   Example:
 ```
 ARCH="X86_64"
 ```

 - Define INSTALLED_DEPENDENCIES/PACKAGES.
 | Note: You can skip this if you want, since this is just for you to keep track of what you install on your VM's System, that way you can target specific Binaries that could've caused *INSTABILITY* or *INIT_ISSUES/ERRORS*
    - `INSTALLED_DEP="LIST"`
    
    Example:
    ```
    INSTALLED_DEP="LIST_OF_YOUR_INSTALLED_PACKAGES \
    USE_BACKSLASH_FOR_NEW_LINES                    \    YOU_CAN_ADD_DATES_HERE
    |___ AND_THE_SOURCE_HERE                       \
    YOU_CAN_EVEN_SET_TAGS_LIKE  REQUIRED           \
    YOU_CAN_JUST_LEAVE_IT_BLANK                    \
    OR_USE_WWW_TAG_FOR_THINGS                      \
    LIKE_A_BROWSER              WWW                \
    "
    ```

- Define MAX_SIZE_DRIVE parameters
| Sometimes you can get to forget what is the maximum size of your virtual drive.
| So set this parameter if you think you might need it.
| This parameter just tells you what size is your virtual drive, you can put any number and SIZE_NUMERATOR, things like B/Bytes, KB/KiB, MB/MiB, GB/GiB, TB/TiB, ETC.
   - `MAX_SIZE_DRIVE="SIZE SIZE_NUMERATOR"`

       Example:
       ```
       MAX_SIZE_DRIVE="64GiB"
       ```

       You can also set the parameter *DRIVE_FORMAT* to remember what kind of *DRIVE_FORMAT* you used for your *VIRTIAL_DISK_FILE*

       SYNTAX is: `DRIVE_FORMAT="TYPE_OF_DEVICE / PARTITION_TABLE / PARTITION_TYPE / BOOT_METHOD"`

       Example:
       ```
       DRIVE_FORMAT="VHD_DEVICE / MBR  / BSD Partition Map / LEGACY_BOOT"
       ```

 | You can (Optionally) set the `HOST_MACHINE` and paste your system parameters/specs, so that way other people can help you solve possible issues.
 | this can contain as many HOST data as you prefer and can be formatted as you prefer.

## Set `<!-- USERLAND --!>` Section.

| USERLAND contains USER_DATA, things like LOGIN_USERS and PASSWRD's, so BE CAREFUL with if you WANT to store these data on the VMInfo.dat file or any other hidden file.
| what matters is that YOU remember where it is.

   - USERLAND Holds the data:
        - NON_ROOT_USR : User that is NOT the ROOT account. (e.g., foo, user, john. Please use quotations.)
        - NON_ROOT_PASSWD : The password for the NON_ROOT_USR.(IN QUOTATION.)

        - _ROOT_PASSWD : Holds the ROOT password, you can keep it anywhere else if you want to, but just REMEMBER where it is located.
        - _SSHD : Holds if the package SSHD/OPENSSH starts when the VM Boots or not, TRUE if the SSH service starts on Boot, FALSE if not.
        - _ROOT_SSHD : holds if _SSHD accepts logging in as the ROOT account, usually disabled by default in most systems.

| That's everything for USERLAND!
| YOU can add as many more USER_DATA as you please, just remember to have some order.

## Set Section `<!--VMSTATE --!>`

- Set SubSection `<!-- STATE:SUBSECTION_ERRORS --!>`
| This subsection should be at the beginning of VMSTATE, that way you can write any errors at the very beginning of your machine's state, so then you can solve them later.

    - Syntax:

    | The syntax of this subsection is simple VMSTATE as a *GENERALIZED* marker, error mark, error info, date and time.

    Example:

    - GENERALIZED_MARKER
    ```
    _VM_STATE="OK_NO_ERR"
    ```

    or
    ```
    _VM_STATE=NUMBER_OF_ERRORS LISTED_ON_SECTION

    ```

| Then we set our error markers.

  For NO errors:
        ```
        _ERR_ON_INSTALL="N/A     \
                    NOERR"
        ```

   For Errors on files:
        ```
        _ERR:@:FILE="/PATH/TO/FILE.SOMETHING" YY:DD:MM @ HH:MM:SS
        ```

   For INIT errors:
        ```
        _INIT:ERR="INFO_SYSTEM_GAVE" >> "POSSIBLE_CAUSE" !>> "POSSIBLE_FIX/MORE_INFO"
        ```

   For KERNEL_ERRORS
            ```
            _ERR_KERNEL:PANIC/CORRUPTION/MISSING
            ```

  If _ERR_KERNEL is a PANIC value, add the following line with the data:
    ```
    _PANIC_CODE="CODE", _PANIC_REASON="SYSTEM_GIVEN_REASON", _OTHER_INFO="Any other info you might have about the issue, if you have the time."
    ```

  if _ERR_KERNEL is a CORRUPTION value, this might just serve to target the source of such KERNEL_PANIC:
    ```
    _PANIC_REASON="SOME_SOFTWARE_YOU_SUSPECT_MIGHT_CAUSE_THE_KERNEL_PANIC"
    _PANIC_INFO="Any other information you might want to keep for later issue avoidance."
    _PANIC_PREVENT_MEASSURES="What you plan to do to avoid such KERNEL_PANIC"
    # Any comments you might want to leave for your future you, example: "This is a false KERNEL_PANIC, reboot and it will boot normally."
    ```

                If _ERR_KERNEL is MISSING:

| leave yourself a comment, maybe try looking for a new kernel to boot off, this is just a NOTE tag to remind you that your kernel is not there anymore.


## Set SubSection `<!-- XORG_IS_CONFIG --!>`
| For this subsection, you might want to set the configurations and states of *XORG* or any *DE*/*WM*/*GUI* you installed on your **VM** system/Your **VM** system has already installed as a *DEFAULT_CONF*/*DEFAULT_BINARY*

- SubSection Example:

    ```
        XORG_IS_SETUP="TRUE / VBOXDRIVER"
        XORG_DISPLAY_IS_WORKING="TRUE"
        XORG_CARD0_IS_DETECTED="TRUE"
        XORG_DISPLAY_0_IS_WORKIN="TRUE"
    ```

| feel free to add, delete, modify or comment this as much as you prefer.

## Set SubSection `<!-- DATETIME_CTL --!>`

| NOTE: DTATIME_CTL can depend from the INET_LINK status on some systems, make sure to double check this info.
| This SubSection only contains **1** parameter, that one being `DATE_TIME_IS_SYNC="VALUE"`
| This value can be either `TRUE` or `FALSE`, depending on if the machine's Time is correctly Synced with the HOST's time.
|| SUBNOTE: Some Systems fix this by changing your [TIMEZONE](https://en.wikipedia.org/wiki/Time_zone) to your correct TimeZone.

## Set SubSection `<!-- INET_LINK --!>`

| The SubSection INET_LINK doesn't depend of ANY software, this is just a GENERALIZED tag for saying "You have internet access." or "You DON'T have internet access."
- This SubSection has to contain the values for:

```
INET_LINK_IS_UP="VALUE"
INET_LINK_IS_WORKING="VALUE"
INET_LINK_TYPE="NETWORK_DEVICE / _BRIDGE_DEVICE|_NO_BRIDGE_DEVICE"
```

- Line `INET_LINK_IS_UP="VALUE"` holds only the values `TRUE` or `FALSE`, if the INET_LINK is UP, means that YOUR HOST_MACHINE does provide a INET device for your **VM**, else if it is NOT UP, means that your HOST_MACHINE does NOT provide a INET device for your **VM**.

- Line `INET_LINK_IS_WORKING="VALUE"` holds the value `TRUE` or `FALSE`, If the `INET_LINK_IS_WORKING` is TRUE, your device should be working as expected, if not, there might be some configuration you wanna do on the GUEST System.

- Line `INET_LINK_TYPE="NETWORK_DEVICE / _BRIDGE_DEVICE / _NO_BRIDGE_DEVICE"`, Holds of the name of the INET_DEVICE and if it is a BRIDGE_DEVICE or NOT.

| It just tells you if other VM's can see this one or not.


## Set SubSection `<!!-- STATE:KERNEL_IS_PANIC --!!>`

| This SubSection is composed by 2 arguments, the `STATE` argument, which tells you what is this SubSection's STATE is telling you about.
| and the second argument being the zone we are looking the STATE at, example, KERNEL_IS_PANIC which should tell you if the KERNEL had PANICS before.
| an example of the SYNTAX for this SubSection is:

```
<!!-- STATE:KERNEL_IS_PANIC --!!>
    KERNEL_IS_PANIC_STATE="VALUE"
    KERNELCONF.KERNEL_IS_PANIC="Summary"
    KERNEL.LOAD.KERNEL_IS_PANIC="VALUE"
<!!-- STATE:KERNEL_IS_PANIC --!!>
```

- Line `KERNEL_IS_PANIC_STATE="VALUE"` holds either `TRUE` or `FALSE`, this tells you if the KERNEL had a PANIC_HALT before, if it is true, you might wanna be careful.
If not, procceed as you prefer.

- Line `KERNELCONF.KERNEL_IS_PANIC="Summary"` is there for ADVANCED users, who probably are running CUSTOM_KERNELS, which could be UNSTABlE, it serves as a Summary of what you modified.

- Line `KERNEL.LOAD.KERNEL_IS_PANIC="VALUE"` Tells you if it has a KERNEL_PANIC when loading itself, or if it procceeds through the kernel normally.

|:notebook: NOTE: this SubSection might be for ADVANCED_USERS only, since filling this without knowledge might result in misleaded solutions, possibly causing more issues.

**After these SubSections, the `<!-- VMSTATE --!>` Section ends.**

|:pencil: Try to add a line to the `<!-- DATELOG --!>` Section every time you EDIT it, so that you can keep track of when issues started, from which boot attempt and what packages did it(For Knowing which packages caused issues, you might want to add the "DATEADDED" bit aside of every package, for more strict following of Issues.)
| A example of this Section is:
```
<!-- DATELOG --!>
    _DATE="YY/MM/DD"
    _END_OF_LOG_PAGE
<!-- DATELOG --!>
```


## Set FINNALSTATE Page.

| The FINALSTATE page tells you things like "How Many Times Booted", "How Many Successful Boot Attempts"

| An Example of the FINALSTATE page is:

```
| FINALSTATE:VMSTATE:STATE="VM_STATE:OK|PANIC|MINOR_ERRORS|SEVERE_SYSTEM_ERRORS
| VM:CAN:BOOT:STATE="TRUE|FALSE"
| KERNEL:STATE:INTEGRITY=OK|COMPROMISED|SLIGHTLY_CORRUPTED|HIGLY_CORRUPTED|SYSTEM_IN_DANGER
| BOOT:COUNT=NUM
| BOOT:SUCCESSFUL:COUNT=NUM
| INSTALLER:STATE= UNUSED|IN_USE|USE_SUSPENDED / OS_NAME:IS:INSTALLED|IN_INSTALLATION_PROCESS|FAILED_INSTALLATION="REASON"|DANGEROUS_STATE|CORRUPTED
```
______________________________________________________________________________

## SIDENOTE [IMPORTANT] :warning:

| This **SYNTAX** was created for a **HUMAN** to write, this way we can avoid possible issues when **AUTO_WRITE** a **VMInfo.dat** file(Possible Security ISSUES).
| Please use this **SYNTAX** with responsability, since if used wrong it could leak important VM Info to the public, make sure to keep your **VMInfo.dat** files on a **SAFE** place and do constant **BACKUPS.**


**ATTE: LillyDev (Local Time Consumer.)**

______________________________________________________________________________
______________________________________________________________________________
# Final Words
This Project started because i wanted to keep track of a BSD Installation on a VM, but didn't knew how to attack a debugger correctly, so i wrote this little handbook for myself in hope this might help me keep track of what i do and how i do, as for "Recommendations" i don't recommend using this on systems/VMs that are highly important(The ones at a JOB, or a SERVER).

This was made for ENTRETAINMENT or SMALL_PRACTICAL_USE only, nothing too serious.
If you recommend this to your Boss, he might fire you, so try to not get fired and keep a good track of errors, events and packages on your VM System.

Have Fun!.
Create.
Learn New Things Everyday.

- JDS (And the J-Project)
