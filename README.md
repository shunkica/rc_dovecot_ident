# Dovecot Client IP for Roundcube
Provides the HTTP client IP during IMAP connect to Dovecot in the form of an IMAP identifier for proper authentication penalty processing.

In reverse proxy configurations the actual client IP is resolved if the request originates from a trusted proxy.


## Installation
The plugin can either be installed via Composer or manually.
If the webmail server allows outgoing requests, Composer is the more comfortable option as it automatically resolves dependencies and simplifies updates.

### Using Composer
⚠️ Note that it is not recommended to run Composer as `root`.
Plugin installation requires script support enabled, as the RoundCube installer needs to move the downloaded plugin to the plugins directory and optionally can enable the plugin automatically.

1. Get [Composer][getcomposer].
1. Within the RoundCube webmail root directory, add a new dependency to the plugin.
   ```shell
   php composer.phar require 'foorschtbar/dovecot_client_ip:^1.0'
   ```
1. Composer may ask whether to enable the plugin. Confirm with `y`.

To update the installed plugin to the latest version, simply run:
```shell
php composer.phar update --no-dev
```

### Manual
1. Download the latest release archive or checkout the latest release branch.
1. Extract the contents into a folder named `dovecot_client_ip` within the `plugins` directory of your RoundCube installation.

There are no external dependencies.

Repeat manual installation for updates.
It may be advisable to keep multiple versions of the plugin in separate folders and use a symlink named `dovecot_client_ip` from within the `plugins` directory to the latest version folder instead.


## Configuration

To enable the plugin, add `dovecot_client_ip` to the `$config['plugins']` array in the RoundCube configuration file.  
When using Composer, the installation routine will ask whether to automatically enable the plugin.

### Options
Copy the configuration variables from `config.inc.php.dist` to your global `config.inc.php` or create a new file named `config.inc.php` in the plugin directory and adjust the settings to your needs.

#### Environment Variables
Configuration can also be overridden using environment variables:

- `DOVECOT_CLIENT_IP_TRUSTED_PROXIES`: Comma-separated list of trusted proxy IP addresses or CIDR ranges
- `DOVECOT_CLIENT_IP_PROXY_ALLOW_PRIVATE_CLIENT_IP`: Set to "1", "true", or "yes" to allow private IP addresses from trusted proxies

Environment variables take precedence over settings in the config files.

### Dovecot IMAP
The following settings need to be applied to the Dovecot IMAP server:
1. Add the IP addresses or networks of your RoundCube webmail servers to the `login_trusted_networks` setting in your Dovecot IMAP configuration.
   This disables authentication penalty processing for the configured webmail server IPs and enables processing of the `X-Originating-IP` ident value with the actual client IP provided by this plugin.
1. Restart Dovecot IMAP.

Afterwards, Dovecot connection logs should show the actual client IP of users instead of the webmail server IP in the `rip` (remote IP) field.

## Credits
The original plugin was created by [Michael Maier](https://gitlab.com/takerukoushirou/roundcube-dovecot_client_ip) and is licensed under the GNU General Public License v3 or higher. I added the network (CIDR) support for the trusted proxies, renamed the plugin to remove the unnecessary `roundcube-` prefix and moved the project to GitHub.
