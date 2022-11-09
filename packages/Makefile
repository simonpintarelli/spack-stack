-include ../Make.user

MAKEFLAGS += --output-sync=recurse

GCC_PREFIX = $$($(SPACK) -e ../compilers/2-gcc find --format '{prefix}' gcc@11)

.PHONY: all .locks

all: gcc-rocm523/generated/env gcc-rocm530/generated/env gcc-cuda/generated/env tools/generated/env

# Make sure spack.lock files are never removed as intermediate files...
.locks: gcc-rocm523/spack.lock gcc-rocm530/spack.lock gcc-cuda/spack.lock tools/spack.lock

gcc-rocm523/config.yaml gcc-rocm530/config.yaml gcc-cuda/config.yaml tools/config.yaml: | store
	$(SPACK) config --scope=user add config:install_tree:root:$(STORE)

gcc-rocm523/compilers.yaml:
	$(SPACK) compiler find --scope=user $(call compiler_bin_dirs, $(GCC_PREFIX))

gcc-rocm530/compilers.yaml:
	$(SPACK) compiler find --scope=user $(call compiler_bin_dirs, $(GCC_PREFIX))

gcc-cuda/compilers.yaml:
	$(SPACK) compiler find --scope=user $(call compiler_bin_dirs, $(GCC_PREFIX))

tools/compilers.yaml:
	$(SPACK) compiler find --scope=user $(call compiler_bin_dirs, $(GCC_PREFIX))

-include ../Make.inc

ifeq (,$(filter clean,$(MAKECMDGOALS)))
include gcc-rocm523/Makefile
include gcc-rocm530/Makefile
include gcc-cuda/Makefile
include tools/Makefile
endif
