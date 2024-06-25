# Ansible tips


## Jinja syntax

Count the lenght of items:
```
- hosts: localhost
  gather_facts: false
  vars:
    a_string: abcd1234
    a_list: ['a', 'short', 'list']
    a_dict: {'a': 'dictionary', 'with': 'values'}
  tasks:
  - debug:
      msg: |
        [
          "a_string length: {{ a_string | length }}",
          "a_list length: {{ a_list | length }}",
          "a_dict length: {{ a_dict | length }}"
        ]
```
