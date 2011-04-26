VERSION=0.1.0
LANG1=sl
LANG2=es
PREFIX1=$(LANG1)-$(LANG2)
PREFIX2=$(LANG2)-$(LANG1)

BASENAME=apertium-$(PREFIX1)

##
##	BUILD TARGETS
##

TARGETS_COMMON =  \
     $(PREFIX1).automorf.bin $(PREFIX1).autobil.bin $(PREFIX1).autogen.bin \
     $(PREFIX1).t1x.bin $(PREFIX1).mode $(PREFIX1).autopgen.bin #$(PREFIX).rlx.bin

# $(PREFIX1).t2x.bin $(PREFIX1).t3x.bin $(PREFIX1).lex.bin 

##
## 	DICTIONARIES (ANALYSER / BILINGUAL / GENERATOR / POSTGEN)
##

$(PREFIX1).automorf.bin: $(BASENAME).$(LANG1).dix
	apertium-validate-dictionary $(BASENAME).$(LANG1).dix
	lt-comp lr $(BASENAME).$(LANG1).dix $@

.deps/$(BASENAME).$(PREFIX1).dix: $(BASENAME).$(PREFIX1).dix lexchoicebil.xsl
	if [ ! -d .deps ]; then mkdir .deps; fi
	xsltproc lexchoicebil.xsl $(BASENAME).$(PREFIX1).dix > $@

$(PREFIX1).autobil.bin: .deps/$(BASENAME).$(PREFIX1).dix
	apertium-validate-dictionary .deps/$(BASENAME).$(PREFIX1).dix
	lt-comp lr .deps/$(BASENAME).$(PREFIX1).dix $@

$(PREFIX1).autogen.bin: $(BASENAME).$(LANG2).dix
	apertium-validate-dictionary $(BASENAME).$(LANG2).dix
	lt-comp rl $(BASENAME).$(LANG2).dix $@

$(PREFIX1).autopgen.bin: $(BASENAME).post-$(LANG2).dix
	apertium-validate-dictionary $(BASENAME).post-$(LANG2).dix
	lt-comp lr $(BASENAME).post-$(LANG2).dix $@


##
##	CONSTRAINT GRAMMAR
##

##$(PREFIX1).rlx.bin: $(BASENAME).$(PREFIX1).rlx
##	cg-comp $(BASENAME).$(PREFIX1).rlx $@


##
##	LEXICAL SELECTION
##

#$(PREFIX1).lex.bin: $(BASENAME).$(PREFIX1).lex
#	cg-comp $(BASENAME).$(PREFIX1).lex $@

##
##	TRANSFER RULES
##

$(PREFIX1).t1x.bin: $(BASENAME).$(PREFIX1).t1x
	apertium-validate-transfer $(BASENAME).$(PREFIX1).t1x
	apertium-preprocess-transfer $(BASENAME).$(PREFIX1).t1x $@

#$(PREFIX1).t2x.bin: $(BASENAME).$(PREFIX1).t2x
#	apertium-validate-interchunk $(BASENAME).$(PREFIX1).t2x
#	apertium-preprocess-transfer $(BASENAME).$(PREFIX1).t2x $@
#
#$(PREFIX1).t3x.bin: $(BASENAME).$(PREFIX1).t3x
#	apertium-validate-postchunk $(BASENAME).$(PREFIX1).t3x
#	apertium-preprocess-transfer $(BASENAME).$(PREFIX1).t3x $@

#
#	MODE FILE(S)
#

$(PREFIX1).mode: modes.xml 
	apertium-gen-modes modes.xml
	cp *.mode modes/

## 
##	FILES WHICH AREN'T BUILD BUT ARE DISTRIBUTED
## 

EXTRA_DIST=$(BASENAME).$(LANG1).dix $(BASENAME).$(PREFIX1).dix \
	   $(BASENAME).$(LANG2).dix \
           $(BASENAME).post-$(LANG2).dix \
	   $(BASENAME).$(PREFIX1).t1x $(BASENAME).$(PREFIX1).t2x \
	   $(BASENAME).$(PREFIX1).t3x\
	   $(PREFIX1).prob \
	   $(BASENAME).$(LANG1).tsx \
	   modes.xml
	
#$(BASENAME).$(LANG1).rlx
	
#           $(LANG1)-tagger-data/README $(LANG2)-tagger-data/README 

apertium_sl_mkdir=$(prefix)/share/apertium/apertium-$(PREFIX1)/
apertium_sl_modesdir=$(prefix)/share/apertium/modes/

apertium_sl_mk_DATA=$(PREFIX1).automorf.bin \
                    $(PREFIX1).autobil.bin  \
                    $(PREFIX1).autogen.bin \
                   $(PREFIX1).autopgen.bin \
		    $(PREFIX1).t1x.bin \
		    $(PREFIX1).prob \
	            $(PREFIX1).mode modes.xml

#		    $(PREFIX1).rlx.bin		  
#		    $(PREFIX1).lex.bin 
#		    $(PREFIX1).t2x.bin 
#		    $(PREFIX1).t3x.bin 

install-data-local:
	apertium-gen-modes modes.xml apertium-$(PREFIX1)
	cp *.mode modes/
	$(INSTALL_DATA) $(PREFIX1).mode $(apertium_sl_modesdir)
	$(INSTALL_DATA) $(BASENAME).$(PREFIX1).t1x $(apertium_sl_mkdir)

#	$(INSTALL_DATA) $(BASENAME).$(PREFIX1).t2x $(apertium_sl_mkdir)
#	$(INSTALL_DATA) $(BASENAME).$(PREFIX1).t3x $(apertium_sl_mkdir)

CLEANFILES = -rf $(TARGETS_COMMON) modes
