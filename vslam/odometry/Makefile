FILENAME = sae_digest
PS_FILE = $(FILENAME).ps
PDF_FILE = $(FILENAME).pdf

# PAPERSIZE = a4
PAPERSIZE = letter

LATEX_FILES = *.dvi *.log *.toc *.tof *.aux *.blg *.lof *.lot *.bbl
CLEAN_FILES = $(LATEX_FILES) *.bak core $(PS_FILE) $(PDF_FILE)
COMPRESS_FILES = *.tex *.bib *.sty *.eps *.ps *.fig *.m *.txt *.pgm *.bst *.cls
UNCOMPRESS_FILES = *.Z *.gz

COMPRESS_DIRS = . Tables Figures
COMPRESS = gzip -q
UNCOMPRESS = gunzip

all: digest


digest:
	latex digest
	@if grep "Warning: Citation" digest.log; then bibtex digest; \
	latex digest; fi;
	@while grep Rerun digest.log; do latex digest; done;

ps: digest
	dvips -t $(PAPERSIZE) -o $(PS_FILE) digest

pdf: digest
	dvips -t $(PAPERSIZE) -Ppdf -j0 -G0 -o $(PS_FILE) digest
	ps2pdf -sPAPERSIZE=$(PAPERSIZE) \
		-dPDFSETTINGS=/prepress \
		-dCompatibilityLevel=1.7 \
		$(PS_FILE) $(PDF_FILE)

clean:
	@for i in $(COMPRESS_DIRS) ; \
	do \
	(if (test -d $$i) ; \
	then cd $$i ; \
	echo "Cleaning $$i" ; \
	rm -f *~ ; \
	rm -f $(CLEAN_FILES) ; \
	fi) \
	done
