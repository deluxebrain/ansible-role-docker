# Role Name: DOCKER

[![Build Status](https://travis-ci.org/deluxebrain/ansible-role-docker.svg?branch=master)](https://travis-ci.org/deluxebrain/ansible-role-docker)

Docker installer for Linux, with control over docker daemon startup.
Optionally installs the following packages:

- docker compose
- kubectl
- kubectx ( dependent on kubectl )
- kubens ( installed with kubectx )

## Requirements

None.

## Role Variables

All of the listed variables are defined in `defaults/main.yml`.
Individual variables can be set or overridden by setting them in a playbook for this role.

- `docker_version`: ( default: latest )
  - docker version to install
- `docker_users`: ( default: [] )
  - users to add to docker group
- `enable_docker_daemon`: ( default: yes )
  - whether to enable and start the docker daemon
  - set to `no` to run docker as a client
- `docker_host`: ( default: "" )
  - use in conjuction with `enable_docker_daemon` to point docker client at a docker host
- `install_docker_compose`: ( default: yes )
  - install docker compose
- `docker_compose_version`: ( default: latest )
  - version of docker compose to install
- `docker_compose_install_dir`: ( default: /usr/local/bin )
  - docker compose install dir
- `install_kubectl`: ( default: yes )
  - install kubectl
- `kubectl_version`: ( default: latest )
  - version of kubectl to install
- `kubectl_install_dir`: ( default: /usr/local/bin )
  - kubectl install dir
- `install_kubectx`: ( default: yes )
  - install kubectx and kubens
- `kubectx_version`: ( default: latest )
  - version of kubectx and kubens to install
- `kubectx_install_path`: ( default : /usr/local/bin/kubectx )
  - kubectx and kubens install path

## Dependencies

None.

## Example Playbook

Example below for the following:

- Installation of specific versions of docker, docker compose and kubectl
- Installation of docker as a client pointing to a specific docker host
- Addition of the `root` user to the docker group

```yaml
- hosts: servers
  roles:
      - deluxebrain.docker
        docker_version: 5:19.03.5*
        enable_docker_daemon: no
        docker_host: "tcp://localhost:2375"
        docker_users:
          - root
        compose_version: 1.25.2
        kubectl_version: 1.17.2
```

## Notes

### Docker and apt

Docker is installed using the `docker-ce` `apt` package.
The following `apt` command can be used to list all available versions:

```sh
apt-cache madison docker-ce
```

### Docker and /etc/profile

The path for kubectx is setup in `/etc/profile`.
Docker by default connects to containers using an interactive non-login shell, and hence by default
`/etc/profile` and hence the path for kubectx will not be set.

The following demonstrates the use of `kubectx` with docker:

```sh
docker ps
$ instance

docker exec -it instance bash --login
$ root@instance:/#
```

## License

MIT / BSD

## Author Information

This role was created in 2020 by [deluxebrain](https://www.deluxebrain.com/).
