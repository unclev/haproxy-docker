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
