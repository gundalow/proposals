
# Docker_Image Module Proposal

## Purpose and Scope

The purpose is to update the existing docker_image module. The updates include expanding the module's capabilities to
match the build, load, pull, push, rmi, and save docker commands and adding support for remote registries.


Docker_image will manage images using docker-py to communicate with either a local or remote API. It will
support API versions >= 1.14. The minimum supported version of docker-py has not been decided.

API connection details will be handled externally in a shared utility module.

## Parameters

Docker_image will support the parameters listed below. API connection parameters will be part of a shared utility 
module as mentioned above.

```
archive_path:
  description:
    - Save image to the provided path. Use with state present to always save the image to a tar archive. If
      intermediate directories in the path do not exist, they will be created. If a matching
      archive already exists, it will be overwritten.
  default: null

config_path:
  description:
    - Path to a custom docker config file. Docker-py defaults to using ~/.docker/config.json.
  default: null

container_limits:
  description:
    - A dictionary of limits applied to each container created by the build process.
      Valid keys include: memory, memswap, cpushares, cpusetcpus.  
  default: null

dockerfile:
  description:
    - Name of dockerfile to use when building an image.
  default: Dockerfile

force:
  description:
    - Use with absent state to un-tag and remove all images matching the specified name. Use with states present and tagged
      to ignore idempotents and take action even when an image already exists. If archive_path is specified, the force option
      will cause an existing archive to be overwritten. 
  default: false

http_timeout:
  description:
    - Timeout for HTTP requests during the image build operation. Provide a positive integer value for the number of
      seconds.
  default: null

load_path:
  description:
    - Use with state present to load a previously saved image. Provide the full path to the image archive file.
  default: null
  
name:
  description:
    - Image name. Name format will be one of: name, repository/name, registry_server:port/name.
      When pushing or pulling an image the name can optionally include the tag by appending ':tag_name'.
  required: true

nocache:
  description:
    - Do not use cache when building an image.
  deafult: false

path:
  description:
    - File path or URL to a context from which to build an image.
  default: null

push:
  description:
    - Use with state present to always push an image to the registry. The image name must contain a repository 
      path and optionally a registry. For example: registry.ansible.com/user_a/repository
  default: false

pull:
  description:
    - When building an image downloads any updates to the FROM image in Dockerfiles.
  default: true

repository:
  description:
    - Use with state tagged to set the repository for the tag.
  default: null

rm:
  description:
    - Remove intermediate containers after build.
  default: true

state:
  description:
    - "absent" - if image exists, unconditionally remove it. Use the force option to un-tag and remove all images
      matching the provided name.
    - "present" - check if an image is present using the provided name and tag. If the image is not present or the 
      force option is used, the image will either be pulled, built or loaded. To build the image, provide a path
      to the context and Dockerfile. To load an image, use load_path to provide a path to an archive file. If no
      path or load_path is provided, the image will be pulled.
    - "tagged" - tag an image into a repository. Use the force option to replace an existing image.     
default: present
choices:
  - absent
  - present
  - tagged

tag:
  description:
    - Used to select an image when pulling. Will be added to the image when pushing, tagging or building. Defaults to
      'latest' when pulling an image. Required when tagging.
  default: latest 

```


## Examples

```
- name: build image
  docker_image:
    path: "/path/to/build/dir"
    name: "my_app"
    tags:
      - v1.0
      - mybuild

- name: force pull an image and all tags
  docker_image:
    name: "my/app"
    force: yes
    tags: all

- name: untag and remove image
  docker_image:
    name: "my/app"
    state: absent
    force: yes

- name: push an image to Docker Hub with all tags
  docker_image:
    name: my_image
    push: yes
    tags: all

- name: pull image from a private registry
  docker_image:
    name: centos
    registry: https://private_registry:8080

```


## Returns

```
{
   changed: True
   check_mode: False
   failed: False 
   action: [ a list of performed actions ] 
   results: {
      < image inspection output >
   }
}
```

## Get Involved

Share your thoughts and ideas in [the open issue](https://github.com/ansible/proposals/issues/1).

Review and contribute to [the code](https://github.com/ansible/docker).

## Author

[Chris Houseknecht] (https://github.com/chouseknecht)

[@chouseknecht](https://twitter.com/chouseknecht)
