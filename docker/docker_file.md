# Docker File Module Proposal

## Purpose and Scope

The purpose of docker_file is to provide for retrieving a file or folder from a container's file system, or 
inserting a file or folder into a container.

Docker_file will manage a container using docker-py to communicate with either a local or remote API. It will
support API versions >= 1.14. The minimum supported version of docker-py has not been decided.

API connection details will be handled externally in a shared utility module similar to how other cloud modules operate.

## Parameters

Docker_file accepts the parameters listed below. API connection parameters will be part of a shared utility module
as mentioned above.

```
dest:
  description:
    - Destination path of copied files. If the destination is a container file system, precede the path with a
      container name or ID + ':'. For example, C(mycontainer:/path/to/file.txt). If the destination path does not
      exist, it will be created. If the destination path exists on a the local filesystem, it will not be overwritten.
      Use the force option to overwrite existing files on the local filesystem.
  default: null
  required: true

follow_link:
  description:
    - Follow symbolic links in the src path. If src is local and file is a symbolic link, the symbolic link, not the 
      target is copied by default. To copy the link target and not the link, set follow_link to true.
  default: false

src:
  description:
    - The source path of file(s) to be copied. If source files are found on the container's file system, precede the
      path with the container name or ID + ':'. For example, C(mycontainer:/path/to/files).
  default: null
  required: yes

```

## Examples

```
- name: Copy files from the local file system to a container's file system
  docker_file:
    src: /tmp/rpm
    dest: "mycontainer:/tmp"
    follow_links: yes

- name: Copy files from the container to the local filesystem and overwrite existing files
  docker_file:
    src: "container1:/var/lib/data"
    dest: /tmp/container1/data
    force: yes
    
```

## Returns

```
{
    files: {
        src: /tmp/rpms,
        dest: mycontainer:/tmp
        files_copied: [
            'file1.txt',
            'file2.jpg'
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
