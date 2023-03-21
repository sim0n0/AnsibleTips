# Display Ansible Facts with Ansible

This Ansible playbook retrieves the `ansible_facts` and displays them.

## Usage

1. Ensure that you have Ansible installed on your system.
2. Create a file named `playbook.yml` and copy the contents below into the file:

    ```
    ---
    - name: Display Ansible Facts
      hosts: all
      gather_facts: yes
      tasks:
        - name: Display Ansible Facts
          debug:
            var: ansible_facts
    ```

3. Run the playbook with the following command:

    ```
    ansible-playbook playbook.yml
    ```

4. The `ansible_facts` should be displayed in the output of the command.

## Explanation of the playbook

- `name: Display Ansible Facts`: name of the task that will display the `ansible_facts`.
- `hosts: all`: specifies all hosts for the execution of the task.
- `gather_facts: yes`: collects the `ansible_facts` before executing the task.
- `debug`: module used to display the `ansible_facts`.
- `var: ansible_facts`: displays the `ansible_facts`.

That's it! You can now use this playbook to display the `ansible_facts` on your hosts managed by Ansible.
