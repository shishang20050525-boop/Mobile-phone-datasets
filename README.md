Smartphone Power Consumption Data Collection Process

The objective of this workflow is to collect high-frequency battery, network, and processor state data from a Xiaomi smartphone using a Windows batch script (via ADB) at 1-minute intervals. This data is intended for building a battery power model.
1. Preparation

Hardware Connection: Connect the Xiaomi smartphone to the PC via USB cable or Wi-Fi debugging.

Software Dependencies: Configure the Android Debug Bridge (ADB) environment (platform-tools).

Phone Settings: Enable "Developer Options" and "USB Debugging" to grant ADB permission to read system logs.

2. Data Acquisition Strategy We evolved the script through multiple iterations (V1 to V15) to resolve permission and formatting issues specific to Xiaomi devices:

Basic Battery Stats: Retrieve Battery Level, Temperature, and Voltage via dumpsys battery.

Current/Amperage Reading (Challenge):

Due to system restrictions, the script directly reads the low-level system file /sys/class/power_supply/bms/current_now.

Key Technique: Measurements must be taken when the battery level is below 95% and not fully charged; otherwise, the system cuts off the current, resulting in a 0 or N/A reading.

Wi-Fi Signal Strength (Challenge):

To handle Xiaomi's specific output format (RSSI: -xx with a space), we utilized grep and sed commands for surgical extraction, preventing misinterpretation of the Network ID.

CPU Load: Capture the output of the top command and automatically calculate the sum of User-space and Kernel-space (Sys) usage.

3. Output & Processing

Format: Data is logged in real-time to a CSV file (Comma-Separated Values).

Calculation: The script automatically sums up the CPU load. Subsequently, a "Power vs. Time" curve (Power Trace) can be generated in Excel using the formula Total Power = Voltage × Current.
