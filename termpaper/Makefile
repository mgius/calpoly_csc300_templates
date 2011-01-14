# Make File for creating Tex documents in Linux with bibliographies in the .bib form.
# Taken from Mark Gius who took it from Phil Nico
#
# Targets of interest:
#
#   ps       --- builds the postscript version of the document
#   pdf      --- builds the pdf version of the document
#   all      --- builds both the pdf and ps versions of the document
#   pspre    --- builds the postscript version of the document and starts
#          	 ghostview to look at it
#   pdfpre   --- builds the pdf version of the document and starts acroread
#          	 to view it
#   clean    --- cleans up intermediate files
#   allclean --- cleans up everything that can be regenerated


SHELL     = /bin/bash
MAIN      = texreport
TEXFILES  = texreport.tex
BIBFILES  = texreport
LATEX     = latex
PS        = dvips
PSFLAGS   = -f -t letter -Ppdf
PSPFLAGS  = -geometry +10+10 -watch
PSVIEWER  = gv
PDFVIEWER = acroread
PDFPFLAGS = -geometry +10+10
EXTRACLEAN= *.bbl *.blg *.toc


.PHONY: all ps pdf pre pspre print clean

all:    ps pdf

ps:     $(MAIN).ps

pdf:    $(MAIN).pdf

pre:    pspre

pspre:  $(MAIN).ps
	$(PSVIEWER) $(PSPFLAGS) $(MAIN).ps &

pdfpre: $(MAIN).pdf
	$(PDFVIEWER) $(PDFPFLAGS) $(MAIN).pdf &

print: $(MAIN).ps
	lpr -h $(MAIN).ps

%.dvi:  %.tex $(TEXFILES)

ifdef BIBFILES
	@echo "  Running LaTeX"
	$(LATEX) $<
	@echo "  Running BibTeX"
	bibtex $(BIBFILES)
endif

	@echo "	Running LaTeX until complete"
	@if latex $< | tee /dev/tty | fgrep -is rerun; then 	\
		echo "	Rerunning LaTeX";			\
		while $(LATEX) $< | fgrep -is rerun; do    \
			echo "  Re-Rerunning LaTeX";            \
		done;                                           \
	fi

%.ps: %.dvi
	$(PS) $(PSFLAGS) $(MAIN).dvi > $(MAIN).ps

%.pdf: %.ps
	gs -q -sDEVICE=pdfwrite -sOutputFile=$@ -dNOPAUSE -dBATCH $<

clean:
	rm -f $(MAIN).dvi $(MAIN).log $(MAIN).aux *~ $(EXTRACLEAN)

allclean: clean
	rm -f $(MAIN).ps $(MAIN).pdf

bibclean: allclean
	rm -f $(MAIN).bbl $(MAIN).blg $(MAIN).synctex.gz 
