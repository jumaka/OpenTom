TOOLS=	espeakdsp dogfeed nxloadmon flkill flmessage nxlight \
	nx_bat_monitor_console nx_bat_monitor \
	power_button editor fonts flchoose

all: $(TOOLS)

install: $(TOOLS)
	cp $(TOOLS) $(TOMDIST)/bin

clean:
	rm -f $(TOOLS)

%: %.c
	$(CC) -o $@ --sysroot=$(ARM_SYSROOT) -I$(ARM_SYSROOT)/usr/include -I$(ROOT)/kernel/include $@.c -lnano-X

%: %.cxx
	$(CXX) -o $@ --sysroot=$(ARM_SYSROOT) -I$(ARM_SYSROOT)/usr/include -I$(ROOT)/kernel/include $@.cxx -lfltk -lfltk_images
