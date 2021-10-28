Role Name
=========

An ansible role to easily format a supervisord config . Used for Shellphish's [iCTF](https://github.com/shellphish/ictf-framework) competition, but can be as generic as wanted, any PRs are welcome.

Requirements
------------

This role templates out a supervisord config, so supervisord should be installed to then actually execute the supervisord script.

Role Variables
--------------

The variable structure closely mimics the supervisord files to be self explanatory. Some reasonable defaults are set to reduce the amount of boilerplate code. Examples can be seen in the `tests/` folder. Most variables match pretty directly the corresponding structure in the ini files.


Dependencies
------------

None

Example Playbook
----------------

A minimal example of a service can be created with the following playbook. A service requires at least a name, a working directory and a command to

```yaml
  tasks:
    - name: install a random file dumper as a supervisord service
      include_role:
        name: lukas_dresel.create_supervisord_config
      vars:
        supervisord_config_path: /tmp/supervisord_config.ini

        inet_http_server:

        programs:
          rand0:
            command: "/bin/bash -c 'while true; do head -n 3 /dev/urandom | tee /tmp/r4Nd0m; sleep 10; done'"
            directory: /tmp
            user: "{{ansible_user_id}}"
            autostart: true
```

This creates the following supervisord config file in `/tmp/supervisord_config.ini:
```
[inet_http_server]
port=127.0.0.1:9001


[supervisord]
nodaemon=True
loglevel=info

[rpcinterface:supervisor]
supervisor.rpcinterface_factory = supervisor.rpcinterface:make_main_rpcinterface


[program:rand0]
command=/bin/bash -c 'while true; do head -n 3 /dev/urandom | tee /tmp/r4Nd0m; sleep 10; done'
directory=/tmp
user=honululu

numprocs=1
autostart=True
autorestart=unexpected
startsecs=10
startretries=3
exitcodes=0
stopsignal=TERM
stopwaitsecs=10
```

License
-------

MIT