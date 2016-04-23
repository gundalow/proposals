# Docker_Export Modules Proposal

## Purpose and Scope

The purpose of docker_export is providing the ability to export a container's entire filesystem as a tar archive.

Docker_export will manage a container using docker-py to communicate with either a local or remote API. It will
support API versions >= 1.14. The minimum supported version of docker-py has not been decided.

API connection details will be handled externally in a shared utility module similar to how other cloud modules operate.

## Parameters

Docker_export accepts the parameters listed below. API connection parameters will be part of a shared utility module
as mentioned above.

```
name:
  description: 
    - Provide a container name or ID to have it's file system exported.
  default: null
  required: true
  
dest:
  description:
    - The path and name of the destination archive.
  default: defaults to the name + '.tar' 
```

## Examples

```
- name: Export container filesystem
  docker_file:
    export: container1
    dest: /tmp/conainer1.tar
```

## Returns

```
{
    changed: true
    export: {
        src: container_name,
        dest: local/path/archive_name.tar
    }
}

```


## Get Involved

Share your thoughts and ideas in [the open issue](https://github.com/ansible/proposals/issues/1).

Review and contribute to [the code](https://github.com/ansible/docker).

## Author

[Chris Houseknecht] (https://github.com/chouseknecht)

[@chouseknecht](https://twitter.com/chouseknecht)
