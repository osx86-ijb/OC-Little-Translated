# ACPI Debugging

## Description

By adding debug code to ***SSDT-xxxx*** Hotpatches, it is possible to see the patch or ACPI working process on the console for debugging.

## Requirements
To use it you must install the two components:

1. **Kext**:
  - Install `ACPIDebug.kext` to `OC\Kexts` and update the config.plist.
2. **Patches**:
  - Add `SSDT-RMDT.aml` to `OC\ACPI` and update the config.plist. It adds the device `RMDT` (RehabMan Debug Trace) to your system.
  - Add debugging code for paramters you want to check to your SSDT(s) (See `SSDT-BKeyQxx-Debug.dsl` for example).
3. Reboot

## Debugging

- Open **Console** and search for **`keyword`** (**`keyword`** as defined in the debug section of your SSDT(s), in this example `ABCD-`)
- Check the console output

## Example

To observe `_PTS` and `_WAK` of `ACPI` receiving `Arg0` after the machine sleeps and wakes up.

- Kext and Patches:

  - **ACPIDebug.kext***
  - ***SSDT-RMDT***
  - ***SSDT-PTSWAK*** -- The patch has built-in parameter passing variables `\_SB.PCI9.TPTS`, `\_SB.PCI9.TWAK`, which are convenient for other patches. See "PTSWAK Sleep and WakeFix"
  - ***SSDT-BKeyQxx-Debug*** - This patch is just an example. Debug code is added within the patch to be able to execute debug code after key response. You can specify the brightness shortcut key, or other keys when you actually use it.

    **Note**: The name change required by the above patch is in the comments of the corresponding patch file.

- Observe the console output results

  - Open the console and search for `ABCD-`
  - Complete the sleep and wake up process once
  - Press the key specified by ***SSDT-BKeyQxx-Debug*** and observe the console output results. In general, the following results are displayed.

    ```
    13:19:50.542733+0800 kernel ACPIDebug: { "ABCD-_PTS-Arg0=", 0x3, }
    13:19:55.541826+0800 kernel ACPIDebug: { "ABCD-_WAK-Arg0=", 0x3, }
    ```

	The result shown above is the value of `Arg0` after the last sleep and wakeup.

## Remarks

- Debug code can be diversified, such as `\RMDT.P1`, `\RMDT.P2`, `\RMDT.P3` and so on, see ***SSDT-RMDT.dsl*** for details
- The above drivers, major patches are from [RehabMan](https://github.com/RehabMan/OS-X-ACPI-Debug)