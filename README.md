# apt-mirror Docker Image

Tag | Dockerfile | Image Layers
----|------------|-------------
`bionic` | [Dockerfile](https://github.com/helphi/Dockerfile-apt-mirror/blob/master/bionic/Dockerfile) | [![](https://images.microbadger.com/badges/image/helphi/apt-mirror:bionic.svg)](https://microbadger.com/images/helphi/apt-mirror:bionic "Get your own image badge on microbadger.com")
`xenial` | [Dockerfile](https://github.com/helphi/Dockerfile-apt-mirror/blob/master/xenial/Dockerfile) | [![](https://images.microbadger.com/badges/image/helphi/apt-mirror:xenial.svg)](https://microbadger.com/images/helphi/apt-mirror:xenial "Get your own image badge on microbadger.com")

# Usage

```sh
docker run -v $PWD/apt-mirror:/var/spool/apt-mirror helphi/apt-mirror:xenial
docker run -v $PWD/apt-mirror:/usr/local/apache2/htdocs -p80:80 httpd:2.4.29-alpine
```

## Change Schedule

```sh
mkdir cron.d

echo "0 3 * * * root /usr/bin/apt-mirror > /var/spool/apt-mirror/cron.log" > cron.d/apt-mirror
chown root:root cron.d/apt-mirror
chmod 644 cron.d/apt-mirror

docker run \
  -v $PWD/apt-mirror:/var/spool/apt-mirror \
  -v $PWD/cron.d:/etc/cron.d \
  helphi/apt-mirror:xenial
```

## Change Mirror Source

```sh
mkdir apt

cat << EOF > apt/mirror.list
############# config ##################
#
# set base_path    /var/spool/apt-mirror
#
# set mirror_path  $base_path/mirror
# set skel_path    $base_path/skel
# set var_path     $base_path/var
# set cleanscript $var_path/clean.sh
# set defaultarch  <running host architecture>
# set postmirror_script $var_path/postmirror.sh
# set run_postmirror 0
set nthreads     20
set _tilde 0
#
############# end config ##############

deb http://cn.archive.ubuntu.com/ubuntu xenial main restricted universe multiverse
deb http://cn.archive.ubuntu.com/ubuntu xenial-security main restricted universe multiverse
deb http://cn.archive.ubuntu.com/ubuntu xenial-updates main restricted universe multiverse
#deb http://cn.archive.ubuntu.com/ubuntu xenial-proposed main restricted universe multiverse
#deb http://cn.archive.ubuntu.com/ubuntu xenial-backports main restricted universe multiverse

deb-src http://cn.archive.ubuntu.com/ubuntu xenial main restricted universe multiverse
deb-src http://cn.archive.ubuntu.com/ubuntu xenial-security main restricted universe multiverse
deb-src http://cn.archive.ubuntu.com/ubuntu xenial-updates main restricted universe multiverse
#deb-src http://cn.archive.ubuntu.com/ubuntu xenial-proposed main restricted universe multiverse
#deb-src http://cn.archive.ubuntu.com/ubuntu xenial-backports main restricted universe multiverse

clean http://cn.archive.ubuntu.com/ubuntu
EOF

docker run \
  -v $PWD/apt-mirror:/var/spool/apt-mirror \
  -v $PWD/apt:/etc/apt \
  helphi/apt-mirror:xenial
```

