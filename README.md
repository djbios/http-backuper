# HttpBackuper
[![PyPI version](https://badge.fury.io/py/http-backuper.svg)](https://badge.fury.io/py/http-backuper)

**HttpBackuper** is a Docker-based backup solution that automates the backup process of Docker containers and local paths.
It is designed to be flexible and easy to use, supporting different backup sources and configurations.

The workflow is simple: HttpBackuper gets the content that needs to be backuped from file, folder or command output, compresses it, and sends it to a specified URL. It can also be scheduled to run backups at custom frequencies.
The ability to send backups to a URL makes it easy to integrate with cloud storage services or other backup solutions such as:
- Amazon S3
- Google Cloud Storage
- Dropbox
- WebDAV
- Telegram (using a bot that posts the backup to a chat and store forever for free)

## Features

- Backup Docker containers' data.
- Backup local paths.
- Supports both container and host-level commands for backups.
- Schedule backups with custom frequencies.
- Send backups to a specified URL.
- Healthcheck.IO integration for backup status monitoring.

## Installation

Install HttpBackuper using pip:

```bash
pip install http_backuper
```

Or directly from the source:

```bash
git clone https://github.com/yourusername/http_backuper.git
cd http_backuper
python setup.py install
```

## Usage

### Configuration

HttpBackuper requires a configuration file to specify the backup sources and settings. By default, it uses
/etc/http_backuper/config.yml, but you can specify a different config file using the -c or --config flag when running
http_backuper.

An example configuration file looks like this:

```yaml
# config.yml

general:
url: "https://example.com"
file_field_name: "backup"
healthchecks_io_api_key: "your_key_here"

backups:

  - name: "backup1"
    schedule: "day"
    sources:
      - path: "/var/lib/mysql"
        container_name: "mysql"
      - command: "mysqldump -u root -p password database"
        container_name: "mysql"
```

This configuration will create a daily backup of the /var/lib/mysql directory from the mysql container and the
result of the mysqldump command, also run in the mysql container.

### Running HttpBackuper

To run HttpBackuper:

```bash
http_backuper
```

If you want to use a different config file:

```bash
http_backuper --config /path/to/your/config.yml
```

To run backups immediately without waiting for the next scheduled time:

```bash
http_backuper --immediately
```

To install HttpBackuper as a Linux service:

```bash
http_backuper --install-service
```

License
HttpBackuper is licensed under the terms of the MIT license. See the LICENSE file for details.