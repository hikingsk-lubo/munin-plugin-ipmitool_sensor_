=head1 CONFIGURATION

The possible wildcard values are the following: fan, temp, volt, watt. So you
would create symlinks to this plugin called ipmitool_sensor_fan,
ipmitool_sensor_temp, and ipmitool_sensor_volt.

  ln -s /usr/share/munin/plugins/ipmitool_sensor_ /etc/munin/plugins/ipmitool_sensor_fan
  ln -s /usr/share/munin/plugins/ipmitool_sensor_ /etc/munin/plugins/ipmitool_sensor_temp
  ln -s /usr/share/munin/plugins/ipmitool_sensor_ /etc/munin/plugins/ipmitool_sensor_volt
  ln -s /usr/share/munin/plugins/ipmitool_sensor_ /etc/munin/plugins/ipmitool_sensor_watt

Add the following to your /etc/munin/plugin-conf.d/munin-node:

Note: When you use this, the threshold provided by the sensor board is not used.

  [ipmitool_sensor*]
      user root
      timeout 20

If you want to use "ipmitool sdr", add the following:

  [ipmitool_sensor*]
      user root
      timeout 20
      env.ipmitool_options sdr

Note: I find it useful to use 'sdr list full' since it is much faster

Configurable variables

  ipmitool            - ipmitool command (default: ipmitool)
  ipmitool_options    - ipmitool command options (default: sensor)
                        sdr: you can use 'sdr' instead of sensor.
  cache_file          - cache file
                        (default: /var/lib/munin/plugin-state/plugin-ipmitool_sensor.cache)
  cache_expires       - cache expires (default: 275)

  fan_type_regex      - Regular expression for unit of fan (default: RPM)
  temp_type_regex     - Regular expression for unit of temp (default: degrees C)
  volt_type_regex     - Regular expression for unit of volt (default: Volts)
  watt_type_regex     - Regular expression for unit of volt (default: Watts)

  fan_warn_percent    - Percentage over mininum for warning (default: 5)
  fan_lower_critical  - Preferred lower critical value for fan
  fan_upper_critical  - Preferred upper critical value for fan
  temp_lower_critical - Preferred lower critical value for temp
  temp_lower_warning  - Preferred lower warining value for temp
  temp_upper_warning  - Preferred upper warning value for temp
  temp_upper_critical - Preferred upper critical value for temp
  volt_warn_percent   - Percentage over mininum/under maximum for warning
                        Narrow the voltage bracket by this. (default: 20)
  watt_warn_percent   - Percentage over mininum/under maximum for warning
                        Narrow the wattage bracket by this. (default: 30)

=head1 AUTHOR

Copyright (C) 2008 - 2013 Jun Futagawa (jfut)

=head1 LICENSE

GPLv2

=head1 MAGIC MARKERS

#%# family=manual
##%# capabilities=autoconf suggest

=head1 HOW TO TEST

  cache_file=ipmitool_sensor_ cache_expires=-1 ./ipmitool_sensor_volt
  cache_file=ipmitool_sensor_ cache_expires=-1 ./ipmitool_sensor_volt config
  cache_file=ipmitool_sensor_ cache_expires=-1 ./ipmitool_sensor_volt suggest
  cache_file=ipmitool_sensor_ cache_expires=-1 ./ipmitool_sensor_volt autoconf
  fan_warn_percent=50 fan_lower_critical=100 fan_upper_critical=1000 \
    cache_file=ipmitool_sensor_ cache_expires=-1 ./ipmitool_sensor_fan config
  temp_lower_warning=1 temp_lower_critical=2 temp_upper_critical=71 temp_upper_warning=72 \
    cache_file=ipmitool_sensor_ cache_expires=-1 ./ipmitool_sensor_temp config
  volt_warn_percent=50 \
    cache_file=ipmitool_sensor_ cache_expires=-1 ./ipmitool_sensor_volt config

=head1 TEST DATA

  unr  Upper Non-Recoverable
  ucr  Upper Critical
  unc  Upper Non-Critical
  lnc  Lower Non-Critical
  lcr  Lower Critical
  lnr  Lower Non-Recoverable

=head2 IPMITOOL SENSOR

  # HP ProLiant ML110 G5
  CPU FAN          | 1434.309   | RPM        | ok    | 5537.099  | 4960.317  | 4859.086  | na        | 937.383   | na
  SYSTEM FAN       | 1497.454   | RPM        | ok    | 5952.381  | 5668.934  | 5411.255  | na        | 937.383   | na
  System 12V       | 12.152     | Volts      | ok    | na        | na        | na        | na        | na        | na
  System 5V        | 5.078      | Volts      | ok    | na        | na        | na        | na        | na        | na
  System 3.3V      | 3.271      | Volts      | ok    | na        | na        | na        | na        | na        | na
  CPU0 Vcore       | 1.127      | Volts      | ok    | na        | na        | na        | na        | na        | na
  System 1.25V     | 1.254      | Volts      | ok    | na        | na        | na        | na        | na        | na
  System 1.8V      | 1.842      | Volts      | ok    | na        | na        | na        | na        | na        | na
  System 1.2V      | 1.107      | Volts      | ok    | na        | na        | na        | na        | na        | na
  CPU0 Diode       | na         | degrees C  | na    | na        | 20.000    | 25.000    | 85.000    | 90.000    | 95.000
  CPU0 Dmn 0 Temp  | 24.500     | degrees C  | ok    | na        | 0.000     | 0.000     | 97.000    | 100.000   | 100.500
  CPU0 Dmn 1 Temp  | 29.000     | degrees C  | ok    | na        | 0.000     | 0.000     | 97.000    | 100.000   | 100.500
  # HP ProLiant DL160
  FAN1 ROTOR1      | 7680.492   | RPM        | ok    | na        | inf       | na        | na        | 1000.400  | na
  # HP ProLiant DL360 G5
  Fan Block 1      | 34.888     | unspecified | nc    | na        | na        | 75.264    | na        | na        | na
  Fan Block 2      | 29.792     | unspecified | nc    | na        | na        | 75.264    | na        | na        | na
  Fan Block 3      | 37.240     | unspecified | nc    | na        | na        | 75.264    | na        | na        | na
  Fan Blocks       | 0.000      | unspecified | nc    | na        | na        | 0.000     | na        | na        | na
  Temp 1           | 40.000     | degrees C  | ok    | na        | na        | -64.000   | na        | na        | na
  Temp 2           | 21.000     | degrees C  | ok    | na        | na        | -64.000   | na        | na        | na
  Temp 3           | 30.000     | degrees C  | ok    | na        | na        | -64.000   | na        | na        | na
  Temp 4           | 30.000     | degrees C  | ok    | na        | na        | -64.000   | na        | na        | na
  Temp 5           | 28.000     | degrees C  | ok    | na        | na        | -64.000   | na        | na        | na
  Temp 6           | na         | degrees C  | na    | na        | na        | 32.000    | na        | na        | na
  Temp 7           | na         | degrees C  | na    | na        | na        | 32.000    | na        | na        | na
  Power Meter      | 214.000    | Watts      | cr    | na        | na        | 384.000   | na        | na        | na
  Power Meter 2    | 220.000    | watts      | cr    | na        | na        | 384.000   | na        | na        | na

=head2 IPMITOOL SDR

  # HP ProLiant ML110 G5
  CPU FAN          | 1434.31 RPM       | ok
  SYSTEM FAN       | 1497.45 RPM       | ok
  System 12V       | 12.10 Volts       | ok
  System 5V        | 5.08 Volts        | ok
  System 3.3V      | 3.27 Volts        | ok
  CPU0 Vcore       | 1.14 Volts        | ok
  System 1.25V     | 1.25 Volts        | ok
  System 1.8V      | 1.84 Volts        | ok
  System 1.2V      | 1.11 Volts        | ok
  CPU0 Diode       | disabled          | ns
  CPU0 Dmn 0 Temp  | 23.50 degrees C   | ok
  CPU0 Dmn 1 Temp  | 29 degrees C      | ok
  # HP ProLiant DL360 G5
  Fan Block 1      | 34.89 unspecifi | nc
  Fan Block 2      | 29.79 unspecifi | nc
  Fan Block 3      | 37.24 unspecifi | nc
  Fan Blocks       | 0 unspecified     | nc
  Temp 1           | 41 degrees C      | ok
  Temp 2           | 19 degrees C      | ok
  Temp 3           | 30 degrees C      | ok
  Temp 4           | 30 degrees C      | ok
  Temp 5           | 26 degrees C      | ok
  Temp 6           | disabled          | ns
  Temp 7           | disabled          | ns
  Power Meter      | 208 Watts         | cr
  Power Meter 2    | 210 watts         | cr

