## Makefile for apertium-nio

RELEASE=0.0
VERSION=0.0.0
LANG1=nio
BASENAME=apertium-$(LANG1)

LEXCFLAGS=-v --Werror

TARGETS_COMMON =			\
	$(LANG1).automorf.hfst		\
	$(LANG1).automorf.bin		\
	$(LANG1).automorf.att.gz	\
	$(LANG1).autogen.hfst		\
	$(LANG1).autogen.bin		\
	$(LANG1).autogen.att.gz		\
	$(LANG1).autopgen.bin		\
	$(LANG1).rlx.bin

# This include defines goals for install-modes, .deps/.d, autobil.prefixes and .mode files:
@ap_include@

## nio transducer

.deps/$(LANG1).twol.hfst: $(BASENAME).$(LANG1).twol .deps/.d
	$(HFSTTWOLC) $< -o $@

.deps/$(LANG1).RL.lexc: $(BASENAME).$(LANG1).lexc .deps/.d
	cat $< | grep -v 'Dir/LR' > $@

.deps/$(LANG1).LR.lexc: $(BASENAME).$(LANG1).lexc .deps/.d
	cat $< | grep -v 'Dir/RL' > $@

.deps/$(LANG1).RL.lexc.hfst: .deps/$(LANG1).RL.lexc
	$(HFSTLEXC) $< -o $@ $(LEXCFLAGS)

.deps/$(LANG1).LR.lexc.hfst: .deps/$(LANG1).LR.lexc
	$(HFSTLEXC) $< -o $@ $(LEXCFLAGS)

.deps/$(LANG1).RL.hfst: .deps/$(LANG1).RL.lexc.hfst .deps/$(LANG1).twol.hfst
	$(HFSTCOMPINT) -1 .deps/$(LANG1).RL.lexc.hfst -2 .deps/$(LANG1).twol.hfst -o $@

.deps/$(LANG1).LR.hfst: .deps/$(LANG1).LR.lexc.hfst .deps/$(LANG1).twol.hfst
	$(HFSTCOMPINT) -1 .deps/$(LANG1).LR.lexc.hfst -2 .deps/$(LANG1).twol.hfst -o $@

$(LANG1).autogen.hfst: .deps/$(LANG1).RL.hfst
	$(HFSTF2F) -O $< -o $@

$(LANG1).automorf.hfst: .deps/$(LANG1).LR.hfst
	$(HFSTINV) $< | $(HFSTF2F) -O -o $@

$(LANG1).autogen.att.gz: $(LANG1).autogen.hfst
	$(HFSTF2T) $< | gzip -9 -c -n > $@

$(LANG1).automorf.att.gz: $(LANG1).automorf.hfst
	$(HFSTF2T) $< | gzip -9 -c -n > $@

$(LANG1).autogen.bin: $(LANG1).autogen.att.gz .deps/.d
	zcat < $< > .deps/$(LANG1).autogen.att
	$(LTCOMP) lr .deps/$(LANG1).autogen.att $@

$(LANG1).automorf.bin: $(LANG1).automorf.att.gz .deps/.d
	zcat < $< > .deps/$(LANG1).automorf.att
	$(LTCOMP) lr .deps/$(LANG1).automorf.att $@

$(LANG1).autopgen.bin: $(BASENAME).post-$(LANG1).dix
	$(LTCOMP) lr $< $@

## Debugging transducers (for testvoc)

debug: .deps/$(LANG1).LR-debug.hfst .deps/$(LANG1).lexc.hfst

.deps/$(LANG1).LR-debug.hfst: $(BASENAME).$(LANG1).lexc .deps/.d
	cat $< | grep -v 'Dir/RL' | grep -v 'Use/Circ' > .deps/$(LANG1).LR-debug.lexc
	$(HFSTLEXC) .deps/$(LANG1).LR-debug.lexc -o .deps/$(LANG1).LR-debug.lexc.hfst -v
	$(HFSTCOMPINT) -1 .deps/$(LANG1).LR-debug.lexc.hfst -2 .deps/$(LANG1).twol.hfst -o $@

.deps/$(LANG1).lexc.hfst: .deps/$(LANG1).RL.lexc.hfst
	$(HFSTF2F) -O -i $< -o $@

## Disambiguation rules

$(LANG1).rlx.bin: $(BASENAME).$(LANG1).rlx $(CGCOMP)
	$(CGCOMP) $< $@

## Spell checker

$(LANG1).zhfst: .deps/acceptor.default.hfst .deps/errmodel.default.hfst
	rm -f $@
	zip -j $@ .deps/acceptor.default.hfst .deps/errmodel.default.hfst speller/index.xml

.deps/errmodel.default.hfst: .deps/words.default.hfst .deps/strings.default.hfst
	$(PYTHON) dev/editdist.py -v -s -d 1 -e '@0@' -i speller/editdist.default.txt -a .deps/acceptor.default.hfst \
	| $(HFSTT2F)  -e '@0@' -o .deps/editdist.default.hfst
	$(HFSTDIS) .deps/strings.default.hfst .deps/editdist.default.hfst \
	| $(HFSTMIN) | $(HFSTREP) -f 1 -t 2 -o .deps/editstrings.default.hfst
	$(HFSTDIS) .deps/words.default.hfst .deps/editstrings.default.hfst \
	| $(HFSTF2F)  -f olw -o $@

.deps/words.default.hfst: speller/words.default.txt
	grep -v -e "^#" -e "^$$" $< | $(HFSTS2F)  -j -o $@

.deps/strings.default.hfst: speller/strings.default.txt .deps/anystar.hfst
	grep -v -e "^#" -e "^$$" $< | $(HFSTS2F)  -j | $(HFSTCAT) .deps/anystar.hfst - |\
	$(HFSTCAT) - .deps/anystar.hfst -o $@

.deps/anystar.hfst:
	echo "?*;" | $(HFSTR2F) -S -o $@

.deps/acceptor.default.hfst: $(LANG1).autogen.hfst
	cat $< | $(HFSTF2F) -t | $(HFSTPROJ)  --project=lower | $(HFSTMIN) |$(HFSTF2F)  -f olw -o $@


## Distribution

EXTRA_DIST=$(BASENAME).$(LANG1).lexc		\
		$(BASENAME).$(LANG1).twol	\
		$(BASENAME).$(LANG1).rlx	\
		$(BASENAME).post-$(LANG1).dix	\
		$(LANG1).prob			\
		modes.xml

## Installation stuff
#
#   apertium_nio_dir: This is where the compiled binaries go
#   apertium_nio_srcdir: This is where the source files go

apertium_niodir=$(prefix)/share/apertium/$(BASENAME)/
apertium_nio_srcdir=$(prefix)/share/apertium/$(BASENAME)/

EXTRA_TARGETS=

if HAVE_HFSTOSPELL
EXTRA_TARGETS += $(LANG1).zhfst
endif # HAVE_HFSTOSPELL

apertium_nio_DATA=$(TARGETS_COMMON) $(EXTRA_TARGETS) $(LANG1).prob

pkgconfigdir = $(prefix)/share/pkgconfig
pkgconfig_DATA = $(BASENAME).pc

noinst_DATA=modes/$(LANG1)-morph.mode

install-data-local:
	test -d $(DESTDIR)$(apertium_nio_srcdir) || mkdir -p $(DESTDIR)$(apertium_nio_srcdir)
	$(INSTALL_DATA) $(BASENAME).$(LANG1).lexc $(DESTDIR)$(apertium_nio_srcdir)
	$(INSTALL_DATA) $(BASENAME).$(LANG1).twol $(DESTDIR)$(apertium_nio_srcdir)
	$(INSTALL_DATA) $(BASENAME).$(LANG1).rlx $(DESTDIR)$(apertium_nio_srcdir)
	$(INSTALL_DATA) $(BASENAME).post-$(LANG1).dix $(DESTDIR)$(apertium_nio_srcdir)

## Cleanup

CLEANFILES = $(TARGETS_COMMON)
clean-local:
	-rm -rf .deps modes

## Test

test: all
	echo "TODO test sentence" | apertium -d . nio-morph | tee .test-mt
	@echo "TODO test analysis results" > .test-ref
	@diff .test-ref .test-mt
	@rm .test-ref .test-mt

