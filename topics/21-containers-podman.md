# 21 - Containers with Podman (RHEL)

## Setup
```bash
dnf install podman buildah skopeo
podman version ; podman info
```

## Images
```bash
podman search nginx
podman pull registry.access.redhat.com/ubi9/ubi
podman images
podman rmi IMAGE
podman tag IMAGE myrepo/img:v1
podman push myrepo/img:v1
podman login registry.redhat.io
skopeo inspect docker://docker.io/library/nginx
```

## Containers
```bash
podman run -d --name web -p 8080:80 nginx
podman run -it --rm ubi9 bash
podman ps ; podman ps -a
podman stop web ; podman start web ; podman restart web
podman rm web ; podman rm -f web
podman logs -f web
podman exec -it web bash
podman inspect web
podman stats
podman top web
podman cp file web:/tmp/
```

## Volumes & env
```bash
podman run -d -v /data:/data:Z nginx       # :Z = SELinux relabel
podman volume create vol1
podman run -d -v vol1:/data nginx
podman run -e MYVAR=value nginx
podman run --memory 512m --cpus 1 nginx
```

## Build
`Containerfile`:
```dockerfile
FROM registry.access.redhat.com/ubi9/ubi
RUN dnf install -y httpd && dnf clean all
COPY index.html /var/www/html/
EXPOSE 80
CMD ["httpd", "-DFOREGROUND"]
```
```bash
podman build -t myweb:v1 .
```

## Run as systemd service
```bash
podman generate systemd --name web --files --new
mv container-web.service /etc/systemd/system/
systemctl daemon-reload && systemctl enable --now container-web
```
(RHEL 9 preferred: Quadlet files in `/etc/containers/systemd/`.)

## Rootless / pods
```bash
podman pod create --name mypod -p 8080:80
podman run -d --pod mypod nginx
podman pod ps
loginctl enable-linger alice            # rootless containers survive logout
```
