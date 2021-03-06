
ifndef REPO_HOME
    $(error "Please run 'source ./bin/conf.sh' to setup the project workspace")
endif

BUILD_DIR = $(REPO_BUILD)/spec

OPCODES   = $(REPO_HOME)/doc/opcodes-crypto.tex

SPEC_OUT  = $(BUILD_DIR)/riscv-crypto-spec.pdf
SPEC_TOP  = riscv-crypto-spec.tex
SPEC_TEX  = $(SPEC_TOP) $(wildcard $(REPO_HOME)/doc/tex/*)
SPEC_EXTRA= riscv-crypto-spec.sty $(OPCODES)
SPEC_COMMIT= $(BUILD_DIR)/spec.commit

$(OPCODES) : $(REPO_HOME)/tools/opcodes-crypto
	@mkdir -p $(dir $(OPCODES))
	cat $^ | python3 $(REPO_HOME)/bin/parse_opcodes.py -tex > $@

.PHONY: $(SPEC_COMMIT)
$(SPEC_COMMIT):
	@mkdir -p $(BUILD_DIR)
	@git rev-parse --abbrev-ref HEAD > ${@}
	@echo "@" >> ${@}
	@git log --pretty=format:'%H' -n 1 >> ${@}

$(SPEC_OUT) : $(SPEC_TEX) $(SPEC_EXTRA) $(SPEC_COMMIT)
	@mkdir -p $(BUILD_DIR)
	pdflatex $(basename $(SPEC_TOP))
	bibtex   $(basename $(SPEC_TOP))
	pdflatex $(basename $(SPEC_TOP))
	pdflatex $(basename $(SPEC_TOP))

spec: $(SPEC_OUT)

clean:
	rm -r $(BUILD_DIR)/*
