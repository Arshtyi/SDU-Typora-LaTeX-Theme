base_dir = $(shell pwd)

target_dir = $(base_dir)/latex-theme
working_dir = $(base_dir)/build
linux = linux
typora = typora
pandoc = pandoc

$(shell mkdir -p $(working_dir))
$(shell mkdir -p $(target_dir))

$(shell cp -r $(base_dir)/scss $(working_dir)/scss)
$(shell cp -r $(base_dir)/headers $(working_dir)/headers)

# $(1) is the name of operating system, $(2) is typora or pandoc, $(3) is light or dark theme
define build
	mkdir -p $(target_dir)/$(1)-$(2)
	mkdir -p $(target_dir)/$(1)-$(2)/target
	if [ "$(3)" = "light" ]; then \
		dart-sass --no-source-map --style expanded $(working_dir)/headers/$(1)/$(3).scss $(target_dir)/$(1)-$(2)/target/latex.css; \
	else \
		dart-sass --no-source-map --style expanded $(working_dir)/headers/$(1)/$(3).scss $(target_dir)/$(1)-$(2)/target/latex-$(3).css; \
	fi
endef

# $(1) is the name of operating system
define build-typora
	$(call build,$(1),$(typora),light)
	$(call build,$(1),$(typora),dark)
	cp $(base_dir)/scripts/install.sh $(target_dir)/$(1)-$(typora)
	cp $(base_dir)/README.md $(target_dir)/$(1)-$(typora)
	cp $(base_dir)/../LICENSE $(target_dir)/$(1)-$(typora)
	cp $(base_dir)/../credits.md $(target_dir)/$(1)-$(typora)
	cp -r $(base_dir)/../res $(target_dir)/$(1)-$(typora)/res
endef

define build-pandoc
	$(call build,$(linux),$(pandoc),light)
	$(call build,$(linux),$(pandoc),dark)
endef

.PHONY: all
all: linux

.PHONY: all-and-compress
all-and-compress: all
	cd $(target_dir)/$(linux)-$(typora); zip -r $(target_dir)/latex-theme-$(linux).zip ./*

.PHONY: linux
linux:
	$(call build-typora,$(linux))

.PHONY: pandoc
pandoc:
	$(call build-pandoc,$(linux))

.PHONY: clean
clean:
	rm -rf $(target_dir) $(working_dir) $(base_dir)/.sass-cache
