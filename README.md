# haproxy-docker
Combination of [docker-haproxy-letsencrypt](https://github.com/bringnow/docker-haproxy-letsencrypt) and [letsencrypt-manager](https://github.com/bringnow/docker-letsencrypt-manager) with sample configuration.

- **manager**
```sh
git remote -v
```

```
origin  https://github.com/bringnow/docker-letsencrypt-manager.git (fetch)
origin  https://github.com/bringnow/docker-letsencrypt-manager.git (push)
```

**registering bunch of domain names**

Even though letsencrypt does not issue wildcard certificates, it is posiuble requesting a cert with up to 100 alternative domain names. To simplify the procedure they can be stored in a plan text file, say ``example.tld.list`` containing one domain name per line, and be requested with

```sh
letsencrypt-manager add `cat example.tld.list`
```

Please make sure haproxy responds on each of the domain requests, else the certificate will not be issued.

- **haproxy**
```sh
git remote -v
```

```
origin  https://github.com/bringnow/docker-haproxy-letsencrypt.git (fetch)
origin  https://github.com/bringnow/docker-haproxy-letsencrypt.git (push)
```

```sh
git diff entrypoint.sh
```

```Diff
diff --git a/entrypoint.sh b/entrypoint.sh
index 154c40e..06321fe 100755
--- a/entrypoint.sh
+++ b/entrypoint.sh
@@ -50,6 +50,11 @@ log "/var/lib/haproxy/dev/log exists and is a socket."
 # Launch HAProxy.
 #log $HAPROXY_CMD && print_config
 $HAPROXY_CHECK_CONFIG_CMD
+# if there is command in an interactive terminal - execute it and exit
+if [[ -t 0 && -n "$HAPROXY_USER_PARAMS" ]]; then
+  exec $HAPROXY_USER_PARAMS
+  exit $?
+fi
 $HAPROXY_CMD
 # Exit immidiately in case of any errors or when we have interactive terminal
 if [[ $? != 0 ]] || test -t 0; then exit $?; fi
```
