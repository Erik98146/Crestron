# Mitsubishi Comfort driver for Crestron Home

A Crestron Home (4-Series) driver that controls Mitsubishi HVAC equipment (mini splits) through the
Mitsubishi Comfort (kumo cloud) service, the same one the Comfort phone app uses.
It works with a PAC-USWHS002-WF-1 or -WF-2 Wi-Fi adapter, with or without an MHK2.  It works with multiple zones.

The driver is unofficial. Mitsubishi's cloud API is undocumented and can change, and
the driver is not affiliated with Mitsubishi Electric or Crestron. It needs internet
access from the processor; there is no local (LAN-only) control.

## What you get in Crestron Home

- One **account device** ("Comfort Cloud Account") that signs in and finds your indoor units.
- One **thermostat device per indoor unit**, with a room tile and a page showing:
  room temperature, heat/cool setpoints, power, mode, fan speed, air direction and humidity.
- **Scene and sequence steps** for each unit: Turn Off, Turn On, Set Mode (Heat, Cool,
  Auto, Dry, Fan Only), Set Heat Setpoint and Set Cool Setpoint.

Crestron Home has no native thermostat type for third-party drivers, so each unit is an
"extension" device with its own tile rather than part of the built-in Climate page.

## Install

1. Copy `Platform_Mitsubishi_Comfort_IP.pkg` to the processor folder
   `/user/ThirdPartyDrivers/Import` (Crestron Toolbox File Manager, or SFTP).
   The path is case sensitive.
2. In the Crestron Configure Pro app, add a device and choose the equipment room or other location to hold the driver. Select the driver: **Drivers** > **Platform** > **Mitsubishi Electric** > **Comfort Cloud Account**
3. Enter the email and password you use in the Comfort app. A wrong password is
   reported on this step.
4. Choose Fahrenheit or Celsius, and optionally set a thermostat PIN (see below).
5. The Comfort Cloud units/zones now appear under: **Device Type** > **Managed Platforms**. Add the zones to the appropriate rooms.

## Optional PIN

The account device's settings have a **Thermostat PIN (optional)** field. Leave it blank
for no PIN. If you enter 4 to 8 digits:

- Each thermostat page still shows the temperature and settings, but its controls are
  greyed out and a PIN box appears at the top.
- Entering the PIN unlocks that one unit's page for 2 minutes. Each change you make
  restarts the 2 minutes, and a **Lock now** button locks it again straight away.
- After 5 wrong tries the page refuses any PIN for a minute.
- To change or remove the PIN, edit the account device's settings. If the field cannot be
  left blank there, enter `0` to turn the PIN off.

What the PIN does not cover:

- Scenes, quick actions and sequences still run without it, so schedules keep working.
- The wall thermostat (MHK2) and the Mitsubishi Comfort phone app are not affected.
- The unlock is per unit, not per touch screen or phone: while one person has a unit
  unlocked, it is unlocked on every screen showing that unit.

  ## Troubleshooting

- The account device's page shows the connection status and the last error.
- For driver logs, open a console to the processor (Toolbox Text Console or SSH), run
  `ch logger drivers` to list drivers, then raise this driver's level to `debug`.
- "Sign-in problem": re-enter the account in the device's settings. The driver retries
  every 5 minutes.
- A change can take a few seconds to reach the unit, and Mitsubishi's cloud can report
  the old value for up to a minute. The tile keeps showing what you asked for meanwhile.
- A schedule or hold set on the MHK2 can override a setpoint later, as with the phone app.
