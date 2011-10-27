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

SCRIPTS		= \
    xdg-su

MANPAGES=	$(SCRIPTS:%=man/%.1)
WEBPAGES=	$(SCRIPTS:%=%.html)
XMLFILES=	$(SCRIPTS:%=desc/%.xml)

all:

html:		index.html $(WEBPAGES)

scripts:	README $(SCRIPTS)

man:		$(MANPAGES)

release:	scripts html man
	rm -f xdg-*.in *~ HACKING generate-help-script.awk
	rm -rf desc/
	rm -rf xsl/
	rm -f xdg-file-dialog xdg-su xdg-copy xdg-terminal

distclean: clean
	rm -f Makefile

clean:

scripts-clean:
	rm -f $(SCRIPTS) README man/xdg-*.1 html/xdg-*.html

README: $(SCRIPTS)
	echo > $@
	@for x in $(SCRIPTS) ; do \
	    sh ./$$x --help | grep -B 100 Synopsis | sed -e '$$d' >> $@ ; \
	done

install:
	$(MKDIR) $(DESTDIR)$(bindir)
	for x in $(SCRIPTS) ; do \
	    $(INSTALL_SCRIPT) $$x $(DESTDIR)$(bindir) ; \
	done
	$(MKDIR) $(DESTDIR)$(mandir)
	$(MKDIR) $(DESTDIR)$(mandir)/man1
	for x in $(MANPAGES) ; do \
	    if [ -f $$x ]; then \
	        $(INSTALL_DATA) $$x $(DESTDIR)$(mandir)/man1 ; \
	    fi ; \
	done

uninstall:
	for x in $(MANPAGES) ; do \
	    rm -f $(DESTDIR)$(mandir)/man1/`basename $$x` ; \
	done
	-$(RMDIR) $(DESTDIR)$(mandir)/man1
	-$(RMDIR) $(DESTDIR)$(mandir)
	for x in $(SCRIPTS) ; do \
	    rm -f $(DESTDIR)$(bindir)/$$x ; \
	done
	-$(RMDIR) $(DESTDIR)$(bindir)

%: %.in %.txt
	awk -f generate-help-script.awk $@.in | sed -e 's/@NAME@/'$@'/g' > $@
	chmod a+x $@

xdg-su: xdg-su.in xdg-utils-common.in

man/%.1: desc/%.xml
	(cd man;$(XMLTO) man ../$<)

index.html: $(XMLFILES)
	(cat html/index.html.pre; \
         xsltproc xsl/refentry2htmlindex.xsl $(XMLFILES); \
         cat html/index.html.post ) > html/index.html

%.html: desc/%.xml
	(cd html;$(XMLTO) html-nochunks ../$<)

%.txt: desc/%.xml
	$(XMLTO) txt $<

