SITE_TITLE = "lpcvoid's homepage"
LOCALE = "en_US.utf-8"


POSTS_UNSORTED = $(basename $(notdir $(wildcard blogposts/*.txt)))
#it's sad that make cannot sort in different orders
POSTS := $(shell for w in $(POSTS_UNSORTED); do echo $$w; done | sort -r)

POSTS_LIST = $(addprefix blogposts/, $(addsuffix .txt, $(POSTS)))

# Arguments

BLOGC ?= $(shell which blogc)

OUTPUT_DIR ?= ../
BASE_URL ?= https://lpcvoid.com


DATE_FORMAT = "%Y-%m-%d"
DATE_FORMAT_ATOM = "%Y-%m-%d"

BLOGC_COMMAND = \
	LC_ALL=$(LOCALE) \
	$(BLOGC) \
		-D SITE_TITLE=$(SITE_TITLE) \
		-D BASE_URL=$(BASE_URL) \
	$(NULL) 
	
all: \
	$(OUTPUT_DIR)/index.html \
	$(OUTPUT_DIR)/rss \
	$(OUTPUT_DIR)/blog/index.html \
	$(addprefix $(OUTPUT_DIR)/blog/, $(addsuffix /index.html, $(POSTS)))

$(OUTPUT_DIR)/index.html: $(POSTS_LIST) index.tmpl Makefile
	$(BLOGC_COMMAND) \
		-D DATE_FORMAT=$(DATE_FORMAT) \
		-D FILTER_PER_PAGE=3 \
		-D FILTER_PAGE=1 \
		-D IS_INDEX=1 \
		-l \
		-o $@ \
		-t index.tmpl \
		$(POSTS_LIST)

$(OUTPUT_DIR)/rss: $(POSTS_LIST) rss.tmpl Makefile
	$(BLOGC_COMMAND) \
		-D DATE_FORMAT=$(DATE_FORMAT) \
		-D IS_BLOG_POST_LIST=1 \
		-l \
		-o $@ \
		-t rss.tmpl \
		$(POSTS_LIST)

$(OUTPUT_DIR)/blog/index.html: $(POSTS_LIST) index.tmpl Makefile
	$(BLOGC_COMMAND) \
		-D DATE_FORMAT=$(DATE_FORMAT) \
		-D IS_BLOG_POST_LIST=1 \
		-l \
		-o $@ \
		-t index.tmpl \
		$(POSTS_LIST)

$(OUTPUT_DIR)/blog/%/index.html: blogposts/%.txt index.tmpl Makefile
	$(BLOGC_COMMAND) \
		-D DATE_FORMAT=$(DATE_FORMAT) \
		-D IS_POST=1 \
		-D TAGS=$(TAGS) \
		-o $@ \
		-t index.tmpl \
		$<
