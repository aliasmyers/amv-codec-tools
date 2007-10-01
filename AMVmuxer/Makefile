patch=amvmux_patch.diff

all:
	@echo "usage: make [patch|build|diff|clean|test]"

patch:
	svn revert -R ffmpeg
	svn update ffmpeg
	cd ffmpeg && patch -p0 < ../$(patch)
	ln -sf `pwd`/amvenc.c `pwd`/ffmpeg/libavformat/
	ln -sf `pwd`/amv.h `pwd`/ffmpeg/libavformat/
	cd ffmpeg && ./configure

diff:
	svn diff ffmpeg > $(patch).new
	grep '^[+-]' $(patch).new | grep -v '[+-]Index:' | egrep -v '[+-](+++|----)' && mv $(patch).new $(patch) && svn diff || rm $(patch).new


build:
	cd ffmpeg && make

clean:
	cd ffmpeg && make clean

test: build
	echo "**** test build in gdb ****"
	gdb --args ffmpeg/ffmpeg_g -i hole.avi -f amv -intra -qscale 1 -r 16 -s 160x120 -ac 1 -ar 22050 -y hole.amv

.PHONY: all patch dif build clean test