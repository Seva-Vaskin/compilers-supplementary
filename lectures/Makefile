TEX=xelatex
FLAGS=--shell-escape -interaction=batchmode -halt-on-error
SOURCE=$(sort $(basename $(wildcard *.tex)))
TEMP_SUFFIX=_temp
TEMP_DIR=temp_compile_dir
RM=rm -rf

.PHONY: genpdf $(SOURCE) clean setup_temp_dir

genpdf: setup_temp_dir $(addsuffix $(TEMP_SUFFIX),$(SOURCE))

setup_temp_dir:
	@mkdir -p $(TEMP_DIR)

$(SOURCE): %: %$(TEMP_SUFFIX)
	@:

$(addsuffix $(TEMP_SUFFIX),$(SOURCE)): %$(TEMP_SUFFIX): %.tex
	@echo "Compiling $* with \mathcal instead of \mathscr"
	@# Create a temporary copy with the replacements.
	cp $*.tex $(TEMP_DIR)/$*$(TEMP_SUFFIX).tex
	@# Use sed to replace \mathscr with \mathcal in the temporary file.
	sed -i 's/\\mathscr/\\mathcal/g' $(TEMP_DIR)/$*$(TEMP_SUFFIX).tex
	@# Compile the temporary file in the temporary directory.
	$(TEX) $(FLAGS) -output-directory=$(TEMP_DIR) $(TEMP_DIR)/$*$(TEMP_SUFFIX).tex && $(TEX) $(FLAGS) -output-directory=$(TEMP_DIR) $(TEMP_DIR)/$*$(TEMP_SUFFIX).tex
	@# Move and rename the PDF back to the main directory with the original file name.
	mv $(TEMP_DIR)/$*$(TEMP_SUFFIX).pdf $*.pdf

clean:
	$(RM) *.out *.aux *.log *.fdb_* *.fls *~ auto $(TEMP_DIR)
