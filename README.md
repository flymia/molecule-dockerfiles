![Workflow CI status](https://github.com/flymia/molecule-dockerfiles/actions/workflows/ci.yml/badge.svg)

# molecule-dockerfiles

These two Dockerfiles are used to create Docker images, which can be used to test Ansible roles using a tool called "molecule". The problem is, that Docker images usually don't include systemd, which you need for testing some roles. So these two images here provide a Debian 11 and RockyLinux 8 with systemd, which can be used to testing with molecule.

## Images on the hub

These two images are on the Docker hub and can be used there. There are two versions: arm64 and x86_64.

### Links:
* [Debian 11](https://hub.docker.com/repository/docker/ventor/docker-debian11-ansible)
* [RockyLinux 8](https://hub.docker.com/repository/docker/ventor/docker-rockylinux8-ansible)

## Use them in molecule

For use in molecule you have to reference them the following way in your ```converge.yml```.

````yaml
dependency:
  name: galaxy
driver:
  name: docker
platforms:
  - name: rockylinux
    image: "ventor/docker-${MOLECULE_DISTRO:-rockylinux8}-ansible:latest"
    command: ${MOLECULE_DOCKER_COMMAND:-""}
    privileged: true
    pre_build_image: true
  - name: debian
    image: "ventor/docker-${MOLECULE_DISTRO:-debian11}-ansible:latest"
    command: ${MOLECULE_DOCKER_COMMAND:-""}
    privileged: true
    pre_build_image: true
provisioner:
  name: ansible
verifier:
  name: ansible
````

## References

These two images are used in my ansible roles [flymia.speedtest_go](https://github.com/flymia/ansible-speedtest_go) and [flymia.listmonk](https://github.com/flymia/ansible-listmonk).

## Credits

A big thank you goes to [Jeff Geerling](https://www.github.com/geerlingguy) who provided all these images from the begining. I just modified them according to [this GitHub issue](https://github.com/geerlingguy/docker-ubuntu2004-ansible/issues/18).
