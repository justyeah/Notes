# Installation

> <font color=red>Note: VMware has dropped support for a number of CPUs including early Intel Core i7 CPUs since version 14. Check the [Processor Requirements for Host Systems.](https://docs.vmware.com/en/VMware-Workstation-Pro/14.0/com.vmware.ws.using.doc/GUID-BBD199AA-C346-4334-9F56-5A42F7328594.html) If your CPU is not supported in the newer releases then you can use [vmware-workstation12](https://aur.archlinux.org/packages/vmware-workstation12/)AUR.</font>

* Install the correct dependencies:
  * fuse2 - for vmware-vmblock-fuse
  * gtkmm - for the GUI
  * linux-headers - for module compilation
  * ncurses (ncurses5-compat-libsAUR for older versions of vmware) - needed by the --console installer
  * libcanberra - for event sounds
  * pcsclite
> Download the latest VMware Workstation Pro or Player (or a beta version, if available).

> Start the installation:
```shell {.line-numbers}
# sh VMware-edition-version.release.architecture.bundle
```

~~~
  > Tip: Some useful flags:
  > --eulas-agreed - Skip the EULAs
  > --console - Use the console UI.
  > --custom - Allows changing the install directory to e.g. /usr/local (make sure to update the vmware-usbarbitrator.service paths in #systemd services).
  > -I, --ignore-errors - Ignore fatal errors.
  > --set-setting=vmware-workstation serialNumber XXXXX-XXXXX-XXXXX-XXXXX-XXXXX - Set the serial number during install (good for scripted installs).
  > --required - Only ask mandatory questions (results in silent install when combined with --eulas-agreed and --console).
~~~
* For the System service scripts directory, use /etc/init.d (the default).
> <b>Note:</b> During the installation you will get an error about "No rc*.d style init script directories" being given. This can be safely ignored, since Arch uses systemd.

* Tip: To (re)build the modules from terminal later on, use:
 ```shell {.line-numbers}
 # vmware-modconfig --console --install-all
 ```