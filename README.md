# Tozed G3000A Analysis

A high-performance Wi-Fi 6 Optical Network Terminal (ONT) / Home Gateway Unit (HGU) manufactured by Tozed Kangwei. It is widely distributed by Globe Telecom as the standard issued modem/router for their GFiber broadband plans. It offers a significant upgrade over older Wi-Fi 5 models.

## User levels

The device have 4 user levels:

- User
- Tech
- Super Admin
- Senior Admin

Before the creation of this repository, only the `tech` user was considered by some to be the superadmin, but that's not quite right. In the tech user account, there are even more advanced pages found in superadmin and senior admin — and even a way to enable Telnet. Refer below.

## Enabling Telnet and opening a root shell

This procedure requires authorized access to the ONT. It uses two separate credentials:

1. Find the ONT MAC address.
2. Generate the web administrator password:

	```sh
	python gen_admin.py <MAC>
	```

3. Open `http://192.168.254.254` and sign in as `superadmin` with the generated password.
4. Open the **uPnP** settings, enable **Telnet Configuration**, and wait about 30 seconds.
5. Connect to the Telnet console on port `9876`:

	```sh
	telnet 192.168.254.254 9876
	```

6. At the console login prompt, enter the console username and password `gztzadmin` and `{REDACTED}`.

If authentication succeeds, the console provides a Linux root shell.

## Changing device to EPON

### Persistent

```sh
prolinecmd clear 1
prolinecmd xponmode set EPON
tzupdate -R
```

### Temporary

```sh
tcapi set XPON_LinkCfg Mode 2
tcapi commit XPON
tcapi save
reboot
```

### Verify

```sh
cat /var/romfile.cfg | grep "Mode"
```

It should show Mode 2 (1 is for GPON).

*Copyright Allen Cruiz*