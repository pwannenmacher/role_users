# Deploy-Users

Creates all users defined by vars. Removes all undefined users except the user running ansible.

## Requirements

none

## Role Variables

| Variable               | Required | Default | Choices | Comments                                                                                 |
|------------------------|----------|---------|---------|------------------------------------------------------------------------------------------|
| user_groups            | true      | []      |         | List of user groups that shall exist (may be used globally or at host group level)       |
| additional_user_groups | true      | []      |         | List of additional user groups that shall exist (may be used for each host individually) |
| users                  | true      | {}      |         | List of user objects (see description below)                                             |
| additional_users       | true      | {}      |         | List of additional user objects (see description below)                                  |
| passwordless_sudo      | true      | false   |         | Allow passwordless sudo access for all sudo allowed users                                |

Users have to be defined like this:

```yaml
users:
  - name: username
    comment: Some User
    groups:
      - users
      - admin
    password_hash: "$6$RQJceekL9DN9Z2HL$cKcX5.Ja21cVK/wCDoX21X7Im8KNPo43WLUbJFBNcSuJRUvDwIzj2HaT/oQqNiV8YEjsRaxKLTUHz1zIthe6D1"
    password: "P@$$w0rd"
    password_salt: "S@LT"
    sudo: true
    passwordless_sudo: true
    ssh_authorized_keys:
      - ssh-rsa [...]
```

If `password_hash` is defined, the values in `password` and `password_salt` are ignored.

If neither `password_hash` nor `password` and `password_salt` are defined, the user is created without a usable password (login via SSH key only). Existing passwords are left untouched.

Optional user attributes:

| Attribute     | Default     | Comments                                                                                       |
|---------------|-------------|------------------------------------------------------------------------------------------------|
| shell         | `/bin/bash` | Login shell                                                                                    |
| sudo_commands | `[]`        | Commands the user may run as root without password, written to `/etc/sudoers.d/<name>_commands` |

`sudo_commands` uses sudoers syntax. A trailing `""` forbids any arguments; commas inside a command have to be escaped as `\,`.

Example of a service user that may only trigger a deployment script:

```yaml
additional_users:
  - name: deploy
    comment: CI deployment
    groups: []
    sudo_commands:
      - /opt/apps/ci/deploy.sh ""
    ssh_authorized_keys:
      - ssh-ed25519 [...] gitlab-ci-deploy
```

## Dependencies

Only default modules are used. No dependencies.

## Example Playbook

```yaml
- hosts: all
  become: true
  roles:
    - role: role_users
```

## License

MIT

## Author Information

Paul Wannenmacher
