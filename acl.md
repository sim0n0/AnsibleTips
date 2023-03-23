# Load new acl into device

This is a playbook task for Ansible that allows the user to load a new access control list (ACL) into a Cisco IOS device. The playbook task is implemented using the `ios_config` module.

## Requirements

- Ansible 2.9 or later
- Python 2.7 or later
- A Cisco IOS device with SSH connectivity and a valid set of login credentials

## Usage

1. Add the following task to your playbook:

```yaml
- name: Load new acl into device
  cisco.ios.ios_config:
    lines:
    - 10 permit ip host 192.0.2.1 any log
    - 20 permit ip host 192.0.2.2 any log
    - 30 permit ip host 192.0.2.3 any log
    - 40 permit ip host 192.0.2.4 any log
    - 50 permit ip host 192.0.2.5 any log
    parents: ip access-list extended test
    before: no ip access-list extended test
    match: exact
```

2. Modify the values of the lines, parents, before, and match parameters as needed to match your specific use case.

3. Run your playbook using the ansible-playbook command.

## Parameters :
* lines: The list of ACL lines to be added to the device. Each line should be written as a string.
* parents: The name of the parent ACL to which the new lines will be added.
* before: The line before which the new lines should be added. If not specified, the new lines will be added at the end of the ACL.
* match: The type of matching to be used when searching for the before line. Valid options are exact (default) and line.

