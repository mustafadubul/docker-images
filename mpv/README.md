# MPV

https://mpv.io/

## Example usage

```
docker run --rm -it \
		--privileged \
		--net host \
		--name mpv \
		--group-add audio \
		--group-add video \
		-v ~/Desktop:/videos \
		mpv
```