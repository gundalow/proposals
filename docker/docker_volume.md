# Docker_Volume Modules Proposal

## Purpose and Scope

The purpose of docker_volume is to manage volumes.
 
Docker_volume will use docker-py to communicate with either a local or remote API. It will support API
versions >= 1.14. The minimum supported version of docker-py has not been decided.

API connection details will be handled externally in a shared utility module similar to how other cloud modules operate.
 
## Parameters

Docker_volume accepts the parameters listed below. Parameters for connecting to the API are not listed here, as they
will be part of the shared module mentioned above.

```
driver:
  description:
    - Volume driver.
  default: local
  
force:
  description:
    - Use with state 'present' to force removal and re-creation of an existing volume. This will not remove and
      re-create the volume if it is already in use.

name:
  description:
    - Name of the volume.
  required: true
  default: null

options:
  description:
    - Dictionary of driver specific options. The local driver does not currently support
      any options.
  default: null

state:
  description:
    - "absent" removes a volume. A volume cannot be removed if it is in use.
    - "present" create a volume with the specified name, if the volume does not already exist. Use the force
      option to remove and re-create a volume. Even with the force option a volume cannot be removed and re-created if
      it is in use.
  default: present
  choices:
    - absent
    - present  
```

## Examples

```
- name: Create a volume
  docker_volume:
    name: data

- name: Remove a volume
  docker_volume:
    name: data
    state: absent

- name: Re-create an existing volume
  docker_volume:
    name: data
    state: present
    force: yes
```

## Returns

```
{
    changed: true,
    failed: false,
    action: [ list of actions taken ],
    results: {
        < volume inspection ouptut >
    }
}
```


## Get Involved

Share your thoughts and ideas in [the open issue](https://github.com/ansible/proposals/issues/1).

Review and contribute to [the code](https://github.com/ansible/docker).

## Author

[Chris Houseknecht] (https://github.com/chouseknecht)

[@chouseknecht](https://twitter.com/chouseknecht)

