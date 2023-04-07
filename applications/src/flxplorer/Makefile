NAME=flxplorer
EXT_FLAGS=--sysroot=$(ARM_SYSROOT) -I$(ARM_SYSROOT)/usr/include -I$(ROOT)/kernel/include

install: $(NAME)
	cp $(NAME) $(TOMDIST)/bin

$(NAME): $(NAME).cxx
	$(CXX) -o $@ $(EXT_FLAGS) $(NAME).cxx -L$(ARM_SYSROOT)/usr/lib -lfltk
	$(STRIP) $@

clean:
	rm -f $(NAME) *.o
