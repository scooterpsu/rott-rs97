# Determine which version to build
# Make sure only one of the following is set to 1 at once
# Triple 0 will build the commercial/registered version

SHAREWARE   ?= 0
SUPERROTT   ?= 1
SITELICENSE ?= 0

CPPFLAGS += -DSHAREWARE=$(SHAREWARE)
CPPFLAGS += -DSUPERROTT=$(SUPERROTT)
CPPFLAGS += -DSITELICENSE=$(SITELICENSE)

ROTT ?= rott.elf

# Regular build flags and rules

TOOLCHAIN = /opt/retrogame-toolchain/usr
SDL_CONFIG  = $(TOOLCHAIN)/mipsel-buildroot-linux-uclibc/sysroot/usr/bin/sdl-config

CC = $(TOOLCHAIN)/bin/mipsel-linux-gcc

CFLAGS ?= -g -O2
CFLAGS += -Wall -Wno-unused
CFLAGS += `$(SDL_CONFIG) --cflags`
CFLAGS += $(EXTRACFLAGS)

CPPFLAGS += -DUSE_SDL=1
CPPFLAGS += -DPLATFORM_UNIX=1
CPPFLAGS += $(EXTRACPPFLAGS)

LDFLAGS += $(EXTRALDFLAGS)

LDLIBS += `$(SDL_CONFIG) --libs`
LDLIBS += -lSDL_mixer
LDLIBS += $(EXTRALDLIBS)

OBJS :=
OBJS += cin_actr.o
OBJS += cin_efct.o
OBJS += cin_evnt.o
OBJS += cin_glob.o
OBJS += cin_main.o
OBJS += cin_util.o
OBJS += dosutil.o
OBJS += engine.o
OBJS += isr.o
OBJS += modexlib.o
OBJS += rt_actor.o
OBJS += rt_battl.o
OBJS += rt_build.o
OBJS += rt_cfg.o
OBJS += rt_crc.o
OBJS += rt_com.o
OBJS += rt_debug.o
OBJS += rt_dmand.o
OBJS += rt_door.o
OBJS += rt_draw.o
OBJS += rt_floor.o
OBJS += rt_game.o
OBJS += rt_in.o
OBJS += rt_main.o
OBJS += rt_map.o
OBJS += rt_menu.o
OBJS += rt_msg.o
OBJS += rt_net.o
OBJS += rt_playr.o
OBJS += rt_rand.o
OBJS += rt_scale.o
OBJS += rt_sound.o
OBJS += rt_spbal.o
OBJS += rt_sqrt.o
OBJS += rt_stat.o
OBJS += rt_state.o
OBJS += rt_str.o
OBJS += rt_swift.o
OBJS += rt_ted.o
OBJS += rt_util.o
OBJS += rt_view.o
OBJS += rt_vid.o
OBJS += rt_err.o
OBJS += scriplib.o
OBJS += w_wad.o
OBJS += watcom.o
OBJS += z_zone.o
OBJS += byteordr.o
OBJS += dukemusc.o
OBJS += winrott.o

AUDIOLIB := audiolib/audiolib.a

all: $(ROTT)

$(ROTT): $(OBJS) $(AUDIOLIB)
	$(CC) $(CFLAGS) $(LDFLAGS) $^ $(LDLIBS) -o $@

$(OBJS): develop.h

$(AUDIOLIB):
	$(MAKE) -C audiolib

tidy: 
	$(RM) $(OBJS) $(ROTT) $(ROTT).exe

clean: tidy
	$(MAKE) -C audiolib $@

ipk: $(ROTT)
	@rm -rf /tmp/.rott-ipk/ && mkdir -p /tmp/.rott-ipk/root/home/retrofw/games/rott /tmp/.rott-ipk/root/home/retrofw/apps/gmenu2x/sections/games
	@cp -r rott.elf game/rott.png game/DARKWAR.RTC game/DARKWAR.RTL game/conffiles game/DARKWAR.WAD game/control game/REMOTE1.RTS /tmp/.rott-ipk/root/home/retrofw/games/rott
	@cp game/rott.lnk /tmp/.rott-ipk/root/home/retrofw/apps/gmenu2x/sections/games
	@sed "s/^Version:.*/Version: $$(date +%Y%m%d)/" game/control > /tmp/.rott-ipk/control
	@cp game/conffiles /tmp/.rott-ipk/
	@tar --owner=0 --group=0 -czvf /tmp/.rott-ipk/control.tar.gz -C /tmp/.rott-ipk/ control conffiles
	@tar --owner=0 --group=0 -czvf /tmp/.rott-ipk/data.tar.gz -C /tmp/.rott-ipk/root/ .
	@echo 2.0 > /tmp/.rott-ipk/debian-binary
	@ar r game/rott-retail.ipk /tmp/.rott-ipk/control.tar.gz /tmp/.rott-ipk/data.tar.gz /tmp/.rott-ipk/debian-binary