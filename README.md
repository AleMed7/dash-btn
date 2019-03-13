## Function
* Initiates a home automation task following the press of an unmodified Amazon Dash button.
  * Done by executing a separate script that issues a command-line trigger (IFTTT Webhook via cURL) to toggle the power status of a device while keeping a local log of its present state.

## Purpose
The goal is to bypass heavy dependencies like python, node, and libpcap present in popular projects to provide an alternative that relies solely on shell scripts and basic command-line tools present in a common distribution like OpenWrt. This enables hardware limited embedded devices, such as home routers, to achieve the same functional end as more capable server hardware. Rather than utilizing packet capturing, detecting a button press is accomplished by watching for when the button makes a DHCP request which is already normally logged by the system. The drawback to its simplicity is a lack of focus on security, with compromises made such as the use of HTTP instead of HTTPS in the web request of the trigger script for faster response time and embedded configuration variables for the sake of script brevity.

## Requirements
* OpenWrt base system deployed as primary DHCP server including logread and dnsmasq
* BusyBox compiled with inotifyd selected (inotifywait equivalent)
* GNU find utility (BusyBox find applet not acceptable)

## Setup
* Complete setup of dash buttons to configure wireless credentials and obtain network connectivity.
* Observe the MAC address of each button for later use.
  * Recommended to be recorded along with each button's unique DSN number printed on the back of the case.
* Restrict WAN traffic for each button and optionally later remove from Amazon account.
* Establish a Wi-Fi enabled device and create an associated pair of IFTTT Webhook triggers for on and off functions.
  * The name of each Webhook must be spaced with underscores and suffixed by `_on` and `_off` (e.g. `desk_lamp_on`).
* Configure `/usr/bin/dash-btn` with each button's name and MAC address as demonstrated by the default `log#` and `mac#` variable examples.
  * The name of each button must match a corresponding Webhook pair while being suffixed by `_btn`.
  * If multiple buttons are used to control a single device, append a number in increments (e.g. `desk_lamp_btn1`).
  * State the total number of buttons configured in the `btntotal` variable.
* Set the value of the secret URL key from the IFTTT account being used in `/usr/bin/toggle-smart` in the `key` variable.
* Restart the system or manually run the init script with `/etc/init.d/dash-btn start`.
