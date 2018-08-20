Fail2Ban filter for Ninjafirewall WP+ Edition syslog events
===========================================================

Note: _This will only work if you have full access to your web server's `/etc/fail2ban/` files._

How to use:

* Purchase, install and configure [Ninjafirewall (WP+ Edition)](https://nintechnet.com/ninjafirewall/wp-edition/) to your WordPress web site.
* Enable Firewall Logging in the `Ninjafirewall+ | Firewall Log` menu.
* Check-mark `Write events to the Syslog server too` option.
* Copy [/filter.d/ninjafirewall-syslog.conf](https://github.com/wpkc/fail2ban-filter-ninjafirewall-syslog/blob/master/filter.d/ninjafirewall-syslog.conf) from this repository to `/etc/fail2ban/filter.d/`
* Add a `[ninjafirewall-syslog]` section to the Jails section of your `/etc/fail2ban/jail.local` file...
```python
	[ninjafirewall-syslog]
	port = http,https
	filter = ninjafirewall-syslog
	logpath = %(syslog_ftp)s
	backend = %(syslog_backend)s
	maxretry = 2
	enabled = true
```
* Restart Fail2Ban: `sudo service fail2ban restart`


Additional Notes:

* Keep the `maxretry` override value low; 2 is good, 1 is too low. Attacks are often distributed, with each IP address only being used once. This will create a lot of unnecessary IP bans. Heavier attacks, or smaller bot-nets, will reuse IP addresses, so a setting of 2 will block them.
* If using CloudFlare on the web site, please see the [fail2ban-action-cloudflare-restv4](https://github.com/wpkc/fail2ban-filter-ninjafirewall-wp) repository for an updated CloudFlare action configuration file.

More info: <https://www.kazimer.com/fail2ban-ninjafirewall-wp-plus-syslog-events/>
