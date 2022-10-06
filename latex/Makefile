NAME=cheribsd-getting-started
#BIB=cheri.bib
.PHONY: check-bibliography sort-bibliography all

all:
	latexmk -bibtex -pdflua $(NAME) --jobname=$(NAME)-final -output-directory=build
	cp -f "build/$(NAME)-final.pdf" "$(NAME).pdf"
draft:
	latexmk -bibtex -pdflua $(NAME) --jobname=$(NAME)-draft --output-directory=build-draft
	cp -f "build-draft/$(NAME)-draft.pdf" "$(NAME)-draft.pdf"

clean:
	latexmk -C $(NAME)
	rm -rf build build-draft
	rm -f $(NAME).{aux,log,out,pdf,bbl,markdown.{lua,out}}
	rm -rf _markdown_${NAME}*
