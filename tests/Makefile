MKDIR      	= mkdir -p
RMDIR      	= rmdir
INSTALL         = /bin/install -c $(INSTALL_FLAGS)
INSTALL_PROGRAM = ${INSTALL} $(INSTALL_PROGRAM_FLAGS)
INSTALL_LIBRARY = ${INSTALL} $(INSTALL_PROGRAM_FLAGS)
INSTALL_SCRIPT  = ${INSTALL} $(INSTALL_SCRIPT_FLAGS)
INSTALL_DATA    = ${INSTALL} -m 644 $(INSTALL_DATA_FLAGS)
XMLTO           = /usr/bin/xmlto

prefix          = /usr/local
exec_prefix     = ${prefix}
bindir          = ${exec_prefix}/bin
mandir          = ${prefix}/man

all:

release:
	rm -rf xdg-su tmp
	rm -f xdg-test.log *~

test:
	./testrun -IRC
	rm -rf tmp/xdgt*

distclean: clean
	rm -f Makefile

clean:
	rm -rf tmp/xdgt*

tests-userclean:
	find ${HOME} -name 'xdgtestdata*' | xargs rm -rf
	find ${HOME} -name 'x-xdg*.xml' | xargs rm -f
	find ${HOME}/.kde/share/mimelnk -name 'x-xdg*.desktop' | xargs rm -f

tests-clean:	tests-userclean
	find `echo "${XDG_DATA_DIRS}:/usr/local/share/:/usr/share/:${XDG_CONFIG_DIRS}:/etc/xdg" | sed -e 's/:/ /g'` -name 'xdgtestdata*' | xargs rm -rf
	find `echo "${XDG_DATA_DIRS}:/usr/local/share/:/usr/share/:${XDG_CONFIG_DIRS}:/etc/xdg" | sed -e 's|:| |g'` -name 'x-xdgt*' | xargs rm -f

install:

uninstall:

doc:
	perl doc_gen.pl
