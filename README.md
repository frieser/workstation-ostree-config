# Custom Fedora Workstation ostree config

## Building

```
mkdir -p repo cache
ostree --repo=repo init --mode=archive
```
```
# Build (compose) the variant of your choice
sudo rpm-ostree compose tree --repo=repo --cachedir=cache fedora-silverblue.yaml
```
### Update summary file
```
ostree summary --repo=repo --update
```
## Testing
Instructions to test the resulting build:

    First, serve the ostree repo using an HTTP server. You can use any static file server. For example using https://github.com/TheWaWaR/simple-http-server:


```
simple-http-server --index --ip 192.168.1.XXX --port 8000
```

```
# Add an ostree remote
sudo ostree remote add testremote http://192.168.122.1:8000/repo --no-gpg-verify

# Pin the currently deployed (and probably working) version
sudo ostree admin pin 0

# List refs from variant remote
sudo ostree remote refs testremote

# Switch to your variant
sudo rpm-ostree rebase testremote:fedora/rawhide/x86_64/silverblue

# Reboot and test!
```