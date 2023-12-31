default: help

PATCH         = applyHarness.patch
CONTRACTS_DIR = ../contracts
MUNGED_DIR    = munged

bug_number = `ls tests/participants | wc -l | tr -d ' '`


help:
	@echo "usage:"
	@echo "  make clean:  remove all generated files (those ignored by git)"
	@echo "  make $(MUNGED_DIR): create $(MUNGED_DIR) directory by applying the patch file to $(CONTRACTS_DIR)"
	@echo "  make record: record a new patch file capturing the differences between $(CONTRACTS_DIR) and $(MUNGED_DIR)"
	@echo "  make bug: records a new patch file capturing the differences of $(MUNGED_DIR) between the latest commit and the current state of the work tree"

munged:  $(wildcard $(CONTRACTS_DIR)/*.sol) $(PATCH)
	rm -rf $@
	cp -r $(CONTRACTS_DIR) $@
	patch -p0 -d $@ < $(PATCH)

record:
	diff -druN $(CONTRACTS_DIR) $(MUNGED_DIR) | sed 's+../contracts/++g' | sed 's+munged/++g' > $(PATCH)

.SILENT: bug
bug:
	git diff HEAD -- munged > tests/participants/bug$(bug_number).patch
	
clean:
	git clean -fdX
	touch $(PATCH)
