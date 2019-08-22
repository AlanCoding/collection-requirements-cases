# collection-requirements-cases

use cases for collection requirements

How this works for the reference case:

```
ansible-galaxy collection install -r reference/collections/requirements.yml -p dest
rm -rf dest/ansible_collections/
```

This should run without any problems

### Invalid (does not) Fallback

```
ANSIBLE_CONFIG=fallback/ansible.cfg ansible-galaxy collection install -r fallback/collections/requirements.yml -p dest -vvvv
rm -rf dest/ansible_collections/
```

This does not work.

In other words, it does not fall back to the 2nd server due to a
connection error with the first server.

