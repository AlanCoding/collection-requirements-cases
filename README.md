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
ANSIBLE_CONFIG=fallback_invalid/ansible.cfg ansible-galaxy collection install -r fallback_invalid/collections/requirements.yml -p dest -vvvv
```

This does not work.

In other words, it does not fall back to the 2nd server due to a
"servname not found" error with the first server.

### Not Found Fallback

```
ANSIBLE_CONFIG=fallback_not_found/ansible.cfg ansible-galaxy collection install -r fallback_not_found/collections/requirements.yml -p dest -vvvv
rm -rf dest/ansible_collections/
```

Does not exist:
 - https://galaxy-dev.ansible.com/chrismeyersfsu/tower_modules
 - https://galaxy-dev.ansible.com/chrismeyersfsu/test_things

Exists:
 - https://galaxy.ansible.com/chrismeyersfsu/tower_modules
 - https://galaxy.ansible.com/chrismeyersfsu/test_things

This does work. In this case it tries the dev galaxy. Upon this producing a 404 error,
it tries the release galaxy. The release galaxy server works, and the
requirements are installed from there.

#### Repeating with environment variables

```
ANSIBLE_GALAXY_SERVER_FOO_URL=https://galaxy.ansible.com ANSIBLE_GALAXY_SERVER_BAR_URL=https://galaxy-dev.ansible.com ANSIBLE_GALAXY_SERVER_LIST=bar,foo ansible-galaxy collection install -r fallback_not_found/collections/requirements.yml -p dest -vvvv
rm -rf dest/ansible_collections/
```

This did not work at first, but an issue was filed and the problem was fixed.
Now, for Ansible 2.9, this command works fine.

