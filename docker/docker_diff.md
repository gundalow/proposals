# Docker Diff Modules Proposal

## Purpose and Scope

The docker_diff module will provide a mechanism for retrieving a list of changed files from a container's
file system.

Docker_diff will manage a container using docker-py to communicate with either a local or remote API. It will
support API versions >= 1.14. The minimum supported version of docker-py has not been decided.

API connection details will be handled externally in a shared utility module similar to how other cloud modules operate.

## Parameters

Docker_diff accepts the parameters listed below. API connection parameters will be part of a shared utility module
as mentioned above.

```
name:
  description:
    - Provide one or more container names or IDs. For each container a list of changed files and directories found on the
      container's file system will be returned.
  default: null
  required: true

event_type:
  description:
    - Select the specific event type to list in the diff output.
  choices:
    - all
    - add
    - delete
    - change
  default: all
```

## Examples

```
- name: List all differences for multiple containers.
  docker_diff:
    diff:
      - mycontainer1
      - mycontainer2

- name: Included changed files only in diff output
  docker_diff:
    diff:
      - mycontainer1
    event_type: change
```

## Returns

```
{
    diff: {
        mycontainer1: [
            { state: 'C', path: '/dev' },
            { state: 'A', path: '/dev/kmsg' },
            { state: 'C', path: '/etc' },
            { state: 'A', path: '/etc/mtab' }
        ],
        mycontainer2: [
            { state: 'C', path: '/foo' },
            { state: 'A', path: '/foo/bar.txt' }
        ]
    }
}
```


## Get Involved

Share your thoughts and ideas in [the open issue](https://github.com/ansible/proposals/issues/1).

Review and contribute to [the code](https://github.com/ansible/docker).

## Author

[Chris Houseknecht] (https://github.com/chouseknecht)

[@chouseknecht](https://twitter.com/chouseknecht)
