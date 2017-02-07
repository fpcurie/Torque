All is launched via "crane" (https://github.com/michaelsauter/crane)

1) Build packages
- Create build image (crane provision -v builder/crane.yaml)
- Modify build/Dockerfile with the place you store torque-$VERSION.tar.gz (get from adaptive computing website), and "with-default-server"
- Build package (we are using gitlab-ci with a "docker build -t buildtorque:$CI_BUILD_REF_NAME ." and get artifacts with "docker cp outputs:/rpms $CI_PROJECT_DIR"
- Move packages on a private rpm repo
- Do the same for moab (can't give files here, it's not open source, see maui if you want)

2) Create images
- Change your private rpm repo on {torque,moab,submit}/rootfs/etc/yum.repos.d/torque.repo
- For moab, if you want mysql external database, see ./moab/rootfs/etc/odbc.ini
- Adapt all crane_*.yaml (envs, hostname, and volumes)
We re-use sssd folder from the host to have same auth on all containers

For the first time, you will have to create configs of torque and moab (comment "/opt/moab/etc" volume to see default files, and copy it on NFS server)

"Submit" container is not necessary, but offer a light ssh jump server.

