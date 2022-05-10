![Workflow CI status](https://github.com/flymia/molecule-dockerfiles/actions/workflows/ci.yml/badge.svg)

# molecule-dockerfiles

These two Dockerfiles are used to create Docker images, which can be used to test Ansible roles using a tool called "molecule". The problem is, that Docker images usually don't include systemd, which you need for testing some roles. So these two images here provide a Debian 11 and RockyLinux 8 with systemd, which can be used to testing with molecule.

## Images on the hub

These two images are on the Docker hub and can be used there. There are two versions: arm64 and x86_64.

### Links:
* [Debian 11](https://hub.docker.com/repository/docker/ventor/docker-debian11-ansible)
* [RockyLinux 8](https://hub.docker.com/repository/docker/ventor/docker-rockylinux8-ansible)

## Manual build (x86_64 and arm64)

### Example: Build the Debian 11 version X86_64 and ARM64

````bash
# On ARM64 machine:
cd debian11
docker build -t ventor/docker-debian11-ansible:manifest-arm64 --build-arg ARCH=arm64 .
docker push ventor/docker-debian11-ansible:manifest-arm64

# On X86_64 machine:
cd debian11
docker build -t ventor/docker-debian11-ansible:manifest-amd64 --build-arg ARCH=amd64 .
docker push ventor/docker-debian11-ansible:manifest-amd64

# On any machine:
docker manifest create ventor/docker-debian11-ansible:latest \
--amend ventor/docker-debian11-ansible:manifest-amd64 \
--amend ventor/docker-debian11-ansible:manifest-arm64

docker manifest push ventor/docker-debian11-ansible:latest
````

TODO: Automate that stuff in the actions.

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
