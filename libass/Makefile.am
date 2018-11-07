AM_CFLAGS = -std=gnu99 -Wall -Wextra -Wno-sign-compare -Wno-unused-parameter \
            -Werror-implicit-function-declaration -Wstrict-prototypes        \
            -Wpointer-arith -Wredundant-decls -Wno-missing-field-initializers\
            -D_GNU_SOURCE

LIBASS_LT_CURRENT = 9
LIBASS_LT_REVISION = 2
LIBASS_LT_AGE = 0

SRC_FONTCONFIG = ass_fontconfig.c ass_fontconfig.h

lib_LTLIBRARIES = libass_octopus.la
libass_octopus_la_SOURCES = ass.h ass.c ass_types.h ass_utils.h ass_utils.c \
                    ass_compat.h ass_string.h ass_string.c ass_strtod.c \
                    ass_library.h ass_library.c ass_cache.h ass_cache.c ass_cache_template.h \
                    ass_font.h ass_font.c ass_fontselect.h ass_fontselect.c \
                    ass_render.h ass_render.c ass_render_api.c \
                    ass_parse.h ass_parse.c ass_shaper.h ass_shaper.c \
                    ass_outline.h ass_outline.c ass_drawing.h ass_drawing.c \
                    ass_rasterizer.h ass_rasterizer.c ass_rasterizer_c.c \
                    ass_bitmap.h ass_bitmap.c ass_blur.c ass_func_template.h

libass_octopus_la_LDFLAGS = -no-undefined -version-info $(LIBASS_LT_CURRENT):$(LIBASS_LT_REVISION):$(LIBASS_LT_AGE)
libass_octopus_la_LDFLAGS += -export-symbols $(srcdir)/libass_octopus.sym

if FONTCONFIG
libass_octopus_la_SOURCES += $(SRC_FONTCONFIG)
endif

assheadersdir = $(includedir)/ass
dist_assheaders_HEADERS = ass.h ass_types.h

EXTRA_DIST = libass_octopus.sym
