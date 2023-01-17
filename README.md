# Ansible+Synology: bootstrap `ansible` user

**Rationale**: to have a dedicated account for ansible-powered automation routines only.

This one adds a new `ansible` user with `sudo` permissions & `ssh` enabled.

## Usage

0. An admin account with `ssh` enabled
1. Create a ssh keypair manually
2. Set all variables in `vars` accordingly

E.g.:

``` yaml
  ...

  vars_files:
    - vault.yml
  tasks:
    - import_role:
        name: ansible-role-synology-bootstrap-ansible
      become: yes
      vars:
        ansible_user_public_key: ansible_ssh_ed25519.pub
        ansible_user_password: "{{ secrets.ansible_user_password }}"
      tags:
        - bootstrap

  ...
```

3. Apply the role

Once this is done, `become_user: ansible` could be used for the next ansible tasks (if there are any).

## Notes

- Tested and proven to work on `DSM 7.1`;
- Since the new user's password is more likely never be used directly (or indirectly), consider make it as complex as possible and forget it immediately 🙊;
- Consider to keep the private part of the generated ssh key in [Ansible Vault](https://docs.ansible.com/ansible/latest/vault_guide/index.html);
- The role's package is not idiomatic and this is _mostly_ intentional. You're welcome to contribute back.

## Disclaimer

Since the role is a part of my homelab, it has never been prepared for public distribution and further public support. Though the role suits the needs well, there is no guarantee that it will work for you or will work ever properly. You apply it on your own responsibility.
