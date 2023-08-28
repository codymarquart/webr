WEBR_ROOT = $(abspath ..)
HOST = $(WEBR_ROOT)/host
WASM = $(WEBR_ROOT)/wasm
TOOLS = $(WEBR_ROOT)/tools
BUILD = $(WEBR_ROOT)/R/build

ifndef R_VERSION
  $(error R_VERSION is undefined)
endif

R_SOURCE = $(BUILD)/R-${R_VERSION}
R_HOST_EXE = $(HOST)/R-${R_VERSION}/bin/R
R_WASM_LIB = $(WASM)/R-${R_VERSION}/lib/R/library

#PKG_DEPS = ../../webr-repo/repo/src/contrib/data.table_1.14.7_2bcf868.tar.gz
 # dplyr_1.1.2.tgz tidytext_0.4.1.tgz stringr_1.5.0.tgz tidyr_1.3.0.tgz 
 # udpipe_0.8.11.tgz 
 # 
 # httpuv_1.6.6.9000.tgz cli_3.6.1.tar.gz glue_1.6.2_a404b1c.tar.gz 

# tokenizers: stringi_1.7.12.tar.gz 
# text2vec: float_0.3-1.tar.gz
# mlapi_0.1.1.tar.gz 
# RhpcBLASctl_0.23-42.tar.gz rsparse_0.5.1.tar.gz

PKGS = webr httpuv_1.6.6.9000.tgz \
 data.table_1.14.7.tgz Matrix_1.5-3.tgz jsonlite_1.8.7.tgz R6_2.5.1.tgz \
 matrixStats_1.0.0.tgz \
 ISOcodes_2022.09.29.tgz \
 tokenizers_0.3.0.tgz \
	Rcpp_1.0.11.tgz \
	SnowballC_0.7.1.tar.gz \
 text2vec_0.6.3.tgz \
 	digest_0.6.32.1_ce223b9.tar.gz \
		lattice_0.21-8.tar.gz \
	lgr_0.4.4.tar.gz  \
 rlang_1.1.1.tgz magrittr_2.0.3.tgz later_1.3.0.9000.tgz promises_1.2.0.1.tgz \
 ulid_0.3.0.tgz 

PKG_PATHS = $(patsubst %,$(R_WASM_LIB)/%,$(PKGS))
PKG_SRCS = $(shell find . -type f \! -path \*/.Rproj.user/\* -a \! -name .Rhistory)
PKG_OBJS = $(shell find . -mindepth 2 -type f -name \*.o -o -name \*.so)

all: $(PKG_PATHS)

$(R_WASM_LIB)/%: $(PKG_SRCS)
	@mkdir -p $(R_WASM_LIB)
	WASM_TOOLS="$(TOOLS)" \
	R_SOURCE="$(R_SOURCE)" \
	R_MAKEVARS_USER="$(WEBR_ROOT)/packages/webr-vars.mk" \
	$(R_HOST_EXE) CMD INSTALL --library="$(R_WASM_LIB)" $(notdir $@) \
	  --no-docs \
	  --no-test-load \
	  --no-staged-install \
	  --no-byte-compile

clean:
	rm -rf $(PKG_PATHS)
	rm -f $(PKG_OBJS)
