# ttyrec ttygif

## install

- [references](https://github.com/icholy/ttygif/blob/master/README.md)

```sh
apt install ttyrec ttygif
```

## record tty

```sh
ttyrec
```

now record working until press CTRL+D

this produces a `tty.gif` files

## convert ttyrecord to tty.gif

open a terminal window with size you need

```sh
export WINDOWID=$(xdotool getwindowfocus)
```

now you can create `tty.gif` using

```sh
ttygif ttyrecord
```

## troubleshoot

if you receive `cache resource exhausted` error may need to tune `/etc/ImageMagick-6/policy.xml` file as described in this [issue](https://github.com/ImageMagick/ImageMagick/issues/396#issuecomment-319569255)
