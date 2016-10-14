ifeq (,$(shell sh -c 'cygpath --version 2> /dev/null'))
  # Unix
  pwd := $$(pwd)
  translate = $1
else
  # Windows mit MSys2/Cygwin
  pwd := $$(cygpath -m "$$(pwd)")
  translate = $(shell echo '$1' | sed 's/:/;/g')
endif


all: build/{{cookiecutter.experiment_short_title}}.pdf

# hier Python-Skripte:
build/plot.pdf: plot.py ../common/matplotlibrc ../common/header-matplotlib.tex | build
	MATPLOTLIBRC=../common TEXINPUTS="$(call translate,$(pwd)/../common:)" python plot.py

# hier weitere Abhängigkeiten für build/{{cookiecutter.experiment_short_title}}.pdf deklarieren:
build/{{cookiecutter.experiment_short_title}}.pdf: build/plot.pdf

build/{{cookiecutter.experiment_short_title}}.pdf: FORCE | build
	  TEXINPUTS="$(call translate,build:)" \
	  BIBINPUTS=build: \
	  max_print_line=1048576 \
	latexmk \
	  --lualatex \
	  --output-directory=build \
	  --interaction=nonstopmode \
	  --halt-on-error \
	{{cookiecutter.experiment_short_title}}.tex

build:
	mkdir -p build

clean:
	rm -rf build

FORCE:

.PHONY: all clean
