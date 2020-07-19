# Mount s3fs-fuse

## Compile (Ubuntu)
Installing Dependencies:
```shell script
apt update && apt upgrade -y
apt -y install automake autotools-dev fuse g++ git libcurl4-gnutls-dev libfuse-dev libssl-dev libxml2-dev make pkg-config
```
Installing s3fs-fuse
```shell script
git clone https://github.com/s3fs-fuse/s3fs-fuse.git
cd s3fs-fuse
sed -i 's/MAX_MULTIPART_CNT = 10 /MAX_MULTIPART_CNT = 1 /' src/fdcache.cpp

./autogen.sh
./configure
make
make install

cp ~/s3fs-fuse/src/s3fs /usr/local/bin/s3fs
cp ~/s3fs-fuse/doc/man/s3fs.1 /usr/share.man/man1/
```

## Configuring s3fs
```shell script
echo <ACCESS_KEY>:<SECRET_KEY> >  ~/.passwd-s3fs
chmod 600  ~/.passwd-s3fs
```
Parameters:
- `$SCW-BUCKET-NAME` is the name of your Object Storage bucket
- `$FOLDER-TO-MOUNT` is the local folder to mount it
- `$SCW-URL` is the remote infrastructure. For ScaleWay it is s3.nl-ams.scw.cloud (Amsterdam, The Netherlands) or s3.fr-par.scw.cloud (Paris, France).

```shell script
s3fs $SCW-BUCKET-NAME $FOLDER-TO-MOUNT -o allow_other -o passwd_file=$HOME/.passwd-s3fs -o use_path_request_style -o endpoint=fr-par -o parallel_count=15 -o multipart_size=10 -o nocopyapi -o url=$SCW-URL
```

## Mount on boot
Add in `/etc/fstab`
```shell script
s3fs#[bucket_name] /mount-point fuse _netdev,allow_other,use_path_request_style,url=https://s3.fr-par.scw.cloud/ 0 0
```

## Appendix A - Sources
- [How to use Object Storage with s3fs](https://www.scaleway.com/en/docs/object-storage-with-s3fs/)

## Appendix B - Related Tools
- [How to use Object Storage with s3cmd](https://www.scaleway.com/en/docs/object-storage-with-s3cmd/)
