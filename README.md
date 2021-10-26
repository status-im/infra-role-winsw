# Description

This role configures a Windows service using [WinSW](https://github.com/winsw/winsw) - windows service wrapper.

# Configuration

The are two configuration formats available: [XML](https://github.com/winsw/winsw/blob/v2.11.0/doc/xmlConfigFile.md) and [YAML](https://github.com/winsw/winsw/blob/v2.11.0/doc/yamlConfigFile.md).
We chose YAML because it's more readable for humans and easier to edit.

A bare minimum configuration would look like this:
```yaml
winsw_service_id: 'jenkins-agent'
winsw_service_name: 'Jenkins JNLP'
winsw_service_description: 'JNLP Agent service'
winsw_service_user: 'jenkins'
winsw_service_exe_url: 'https://example.org/jenkins-agent.jar'
winsw_service_exe_sha256: '123qwe123qwe123qwe123qwe132qwe123qwe123q'
winsw_service_arguments:
  - '-jnlpUrl "{{ jnlp_url }}"'
  - '-secret "{{ jnlp_secret }}"'
```
You can also just specify `winsw_service_exe_path` if there's no need to download it.

# Management

Services are managed with a WinSW binary that is a wrapper around the actual service process.
That same binary can be used to install, start, stop, restart, and uninstall the service:
```
admin@windows-01 MINGW64 .../jenkins-agent
$ ls -l
total 25188
-rwxr-xr-x 1 admin 197121   655872 Jul 16 14:37 jenkins-agent.exe*
-rw-r--r-- 1 admin 197121     1254 Jul 16 14:37 jenkins-agent.yml
drw-r--r-- 1 admin 197121        0 Jul 16 14:37 logs

admin@windows-01 MINGW64 .../jenkins-agent
$ ls -l logs
-rw-r--r-- 1 admin 197121        0 Jul 16 14:49 jenkins-agent.err.log
-rw-r--r-- 1 admin 197121 13911763 Jul 16 15:40 jenkins-agent.out.log
-rw-r--r-- 1 admin 197121     2158 Jul 16 14:49 jenkins-agent.wrapper.log

admin@windows-01 MINGW64 .../jenkins-agent
$ ./jenkins-agent.exe stop
2021-07-16 15:41:20,750 INFO  - Stopping service 'Jenkins JNLP (jenkins-agent)'...
2021-07-16 15:41:20,771 INFO  - Service 'Jenkins JNLP (jenkins-agent)' stopped successfully.

admin@windows-01 MINGW64 .../jenkins-agent
$ ./jenkins-agent.exe start
2021-07-16 15:41:24,960 INFO  - Starting service 'Jenkins JNLP (jenkins-agent)'...
2021-07-16 15:41:24,972 INFO  - Service 'Jenkins JNLP (jenkins-agent)' has already started.
```

# Known Issues

Configuring services to run as a regular user with `allowservicelogon: true` does not work currently for some unknown reason. It can be fixed manually by updating the user password in service properties which will trigger granting the "logon" right for the user and will fix any other services using that user.

Issue: https://github.com/winsw/winsw/issues/840
