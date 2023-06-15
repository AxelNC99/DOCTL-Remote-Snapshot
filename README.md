
# Digital Ocean Droplet Remote Snapshot

This script uses Digital Ocean's [DOCTL](https://github.com/digitalocean/doctl) command line interface to connect to a specific Droplet to take a snapshot. The script shuts the droplet down beforehand to prevent data corruption as recommended by Digital Ocean. Once complete, the droplet is returned to a powered on state. Once booted, it will retain a specified number of snapshots (excluding backups) associated with the Droplet's ID as indicated in the configuration file.

Learn about how I can came up with this idea: https://aaronweiss.me/automated-digital-ocean-droplet-snapshots-with-doctl

## Table of Contents
- [Requirements](#requirements)
- [Optional](#optional)
- [Installation](#installation)
- [Usage](#usage)
	* [Configuration](#configuration)
	* [Execution](#execution)
	* [Cron](#cron)
- [Thank Yous](#thank-yous)

## Requirements
- [doctl](https://github.com/digitalocean/doctl#installing-doctl)

## Optional
- 'sendmail' SMTP configured to send email notifications

## Installation
```git clone https://github.com/AxelNC99/DOCTL-Remote-Snapshot.git```

## Usage

### Configuration
Add the following information to your do_droplet.config file
```
dropletid=

# Enter the number of snapshots to keep
numretain=

#Have a notification send to an email
recipientemail=your@email.account

#Optional
#Append an additional note at end of snapshot name. Currently, it's set to "_cron_snapshot"
snap_name_append="_cron_snapshot"
```

`dropletid` is your Droplet's ID. If you do not know your Droplet's ID, log into your [Digital Ocean account ](https://cloud.digitalocean.com/droplets), click on the droplet, and the URL of your droplet will contain your Droplet's ID after the /droplets/ directory, like so: https://cloud.digitalocean.com/droplets/**XXXXXXXXX**/graphs?i=78109b&period=hour

`numretain` is the amount of snapshots you'd like to keep. 

### Execution
`sudo bash auto_snapshot.bash`

### Flags
-r - Cancel any retention. No snapshots will be deleted.

Example:

	`sudo bash auto_snapshot.bash -r `

-p - Prevent the script from powering the droplet off.

Example:

	`sudo bash auto_snapshot.bash -p`

Combining flags:

	`sudo bash auto_snapshot.bash -p -r`

#### Cron
Consider adding this script to your crontab. Below is an example to run this script every Wednesday at 1 AM
```
0 1 * * 3 /bin/bash /usr/local/bin/auto_snapshot.bash -r
```

## Resources
https://github.com/digitalocean/doctl#authenticating-with-digitalocean
https://www.digitalocean.com/community/tutorials/how-to-use-doctl-the-official-digitalocean-command-line-client


