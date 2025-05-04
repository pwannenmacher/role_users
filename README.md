# Deploy-Users

Creates all users defined by vars. Removes all undefined users except the user running ansible.

## Requirements

none

## Role Variables

| Variable               | Required | Default | Choices | Comments                                                                                 |
|------------------------|----------|---------|---------|------------------------------------------------------------------------------------------|
| user_groups            | yes      | []      |         | List of user groups that shall exist (may be used globally or at host group level)       |
| additional_user_groups | yes      | []      |         | List of additional user groups that shall exist (may be used for each host individually) |
| users                  | yes      | {}      |         | List of user objects (see description below)                                             |
| additional_users       | yes      | {}      |         | List of additional user objects (see description below)                                  |
| passwordless_sudo      | yes      | false   |         | Allow passwordless sudo access for all sudo allowed users                                |

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

## Dependencies

Only default modules are used. No dependencies.

## Example Playbook

```yaml
- hosts: all
  become: true

  roles:
    - role: deploy_users
```

## License

MIT

## Author Information

Paul Wannenmacher
