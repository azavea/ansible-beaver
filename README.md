# ansible-beaver

An Ansible role for installing [Beaver](https://github.com/josegonzalez/python-beaver).

## Role Variables

- `beaver_version` - Beaver version
- `beaver_conf_dir` - Directory for Beaver configuration stanzas (default: `/etc/beaver`)
- `beaver_data_dir` - Home directory for the Beaver system user (default: `/var/lib/beaver`)
- `beaver_redis_url` - A Redis URL for Beaver to publish messages (default: `redis://localhost:6379/0`)
- `beaver_redis_namespace` - A namespace within Redis for Beaver to publish messages (default: `logstash`)
- `beaver_logstash_version` - Logstash 1.2 introduced a JSON schema change, so if you're using > 1.0, this needs to be set to `1` (default: `1`)
- `beaver_dependencies` - A list of Beaver dependencies that do not install correctly on their own (default: `[ docutils, python-daemon ]`)
- `beaver_log` - Beaver log path (default: `/var/log/beaver.log`)
- `beaver_log_rotate_count` - Beaver log rotation count (default: `5`)
- `beaver_log_rotate_interval` - Beaver log rotation interval (default: `daily`)

## Example Playbook

It is important to note that this role does not include any templates for Beaver stanza configuration. Because Beaver supports `conf.d` style configuration, users of the role are encouraged to write separate Ansible tasks and templates to configure Beaver for their needs.

There is an example template in the [examples](./examples/) directory that reads `/var/log/syslog` with Beaver and publishes new messages to Redis. From there, Logstash pulls the messages from Redis and pretty prints a dump of that to `stdout`. Because Logstash is running as a daemon, `stdout` ends up being `/var/log/upstart/logstash.log`.

Running the following command should allow you to confirm that Beaver, Redis, and Logstash are on the same page:

```bash
$ sudo cat /var/log/upstart/logstash.log
```
