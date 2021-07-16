# Description

This role configures a Windows service using [WinSW](https://github.com/winsw/winsw) - windows service wrapper.

# Configuration

Theare two configuration formats available: [XML](https://github.com/winsw/winsw/blob/v2.11.0/doc/xmlConfigFile.md) and [YAML](https://github.com/winsw/winsw/blob/v2.11.0/doc/yamlConfigFile.md).
We chose YAML because it's more readable for humans and easier to edit.

A bare minimum configuration would look like this:
```yaml
winsw_service_id: 'jenkins-agent'
winsw_service_name: 'Jenkins Agent'
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

TODO

# Known Issues

Configuring services to run as a regular user with `allowservicelogon: true` does not work currently for some unknown reason. It can be fixed manually by updating the user password in service properties which will trigger granting the "logon" right for the user and will fix any other services using that user.

Issue: https://github.com/winsw/winsw/issues/840
