AC_INIT(libass-octopus, 0.14.0)
AM_INIT_AUTOMAKE
AC_CONFIG_MACRO_DIR([m4])
# Disable Fortran checks
define([AC_LIBTOOL_LANG_F77_CONFIG], [:])
LT_INIT
AC_CONFIG_SRCDIR([libass/ass.c])
AC_CONFIG_HEADER([config.h])

# Checks for programs.
AC_PROG_CC
AC_PROG_CPP
AM_PROG_CC_C_O
AM_PROG_AS

# Checks for header files.
AC_HEADER_STDC
AC_HEADER_STDBOOL
AC_CHECK_HEADERS([stdint.h iconv.h])

# Checks for library functions.
AC_CHECK_FUNCS([strdup strndup])

# Checks for libraries.
# Add them to pkg-config for static linking.
OLDLIBS="$LIBS"
LIBS=
use_libiconv=false
AC_SEARCH_LIBS([libiconv_open], [iconv], use_libiconv=true)
AC_SEARCH_LIBS([iconv_open], [iconv], use_libiconv=true)
AC_CHECK_LIB([m], [fabs])
pkg_libs="$LIBS"
LIBS="$OLDLIBS $LIBS"

# Check for libraries via pkg-config
AC_ARG_ENABLE([fontconfig], AS_HELP_STRING([--disable-fontconfig],
    [disable fontconfig support @<:@default=enabled@:>@]))
AC_ARG_ENABLE([emscripten], AS_HELP_STRING([--disable-emscripten],
    [disable emscripten font provider support @<:@default=enabled@:>@]))
AC_ARG_ENABLE([require-system-font-provider], AS_HELP_STRING([--disable-require-system-font-provider],
    [allow compilation even if no system font provider was found @<:@default=enabled:>@]))
AC_ARG_ENABLE([harfbuzz], AS_HELP_STRING([--disable-harfbuzz],
    [disable HarfBuzz support @<:@default=check@:>@]))
AC_ARG_ENABLE([large-tiles], AS_HELP_STRING([--enable-large-tiles],
    [use larger tiles in the rasterizer (better performance, slightly worse quality) @<:@default=disabled@:>@]))

AM_CONDITIONAL([ENABLE_LARGE_TILES], [test x$enable_large_tiles = xyes])

AM_COND_IF([ENABLE_LARGE_TILES],
    [AC_DEFINE(CONFIG_LARGE_TILES, 1, [use large tiles])]
    [AC_DEFINE(CONFIG_LARGE_TILES, 0, [use small tiles])],
    )

PKG_CHECK_MODULES([FREETYPE], freetype2 >= 9.10.3, [
    CFLAGS="$CFLAGS $FREETYPE_CFLAGS"
    CXXFLAGS="$CFLAGS $FREETYPE_CFLAGS"
    LIBS="$LIBS $FREETYPE_LIBS"
    AC_DEFINE(CONFIG_FREETYPE, 1, [found freetype2 via pkg-config])
    ])

PKG_CHECK_MODULES([FRIBIDI], fribidi >= 0.19.0, [
    CFLAGS="$CFLAGS $FRIBIDI_CFLAGS"
    LIBS="$LIBS $FRIBIDI_LIBS"
    AC_DEFINE(CONFIG_FRIBIDI, 1, [found fribidi via pkg-config])
    ])

if test x$enable_fontconfig != xno; then
PKG_CHECK_MODULES([FONTCONFIG], fontconfig >= 2.10.92, [
    CFLAGS="$CFLAGS $FONTCONFIG_CFLAGS"
    LIBS="$LIBS $FONTCONFIG_LIBS"
    AC_DEFINE(CONFIG_FONTCONFIG, 1, [found fontconfig via pkg-config])
	fontconfig=true
    ], [fontconfig=false])
fi
AM_CONDITIONAL([FONTCONFIG], [test x$fontconfig = xtrue])

if test x$enable_harfbuzz != xno; then
PKG_CHECK_MODULES([HARFBUZZ], harfbuzz >= 0.9.5, [
    CFLAGS="$CFLAGS $HARFBUZZ_CFLAGS"
    LIBS="$LIBS $HARFBUZZ_LIBS"
    AC_DEFINE(CONFIG_HARFBUZZ, 1, [found harfbuzz-ng via pkg-config])
	harfbuzz=true
    ], [harfbuzz=false])
fi

# add packages to pkg-config for static linking
if test "$use_libiconv" = true; then
    AC_DEFINE(CONFIG_ICONV, 1, [use iconv])
    if test x"$ac_cv_search_libiconv_open" != x"none required" &&
       test x"$ac_cv_search_iconv_open" != x"none required"; then
        pkg_libs="${pkg_libs} -liconv"
    fi
fi
pkg_requires="freetype2 >= 9.10.3"
pkg_requires="fribidi >= 0.19.0, ${pkg_requires}"
if test x$fontconfig = xtrue; then
    pkg_requires="fontconfig >= 2.10.92, ${pkg_requires}"
fi
if test x$harfbuzz = xtrue; then
    pkg_requires="harfbuzz >= 0.9.5, ${pkg_requires}"
fi

if test x$enable_require_system_font_provider != xno &&
   test x$fontconfig != xtrue
then
    AC_MSG_ERROR([\Fontconfig is required. If you really want to compile without \
a font provider, add --disable-require-system-font-provider])
fi

AC_SUBST([PKG_LIBS_DEFAULT], [$(test x$enable_shared = xno && echo ${pkg_libs})])
AC_SUBST([PKG_REQUIRES_DEFAULT], [$(test x$enable_shared = xno && echo ${pkg_requires})])
AC_SUBST([PKG_LIBS_PRIVATE], [$(test x$enable_shared = xno || echo ${pkg_libs})])
AC_SUBST([PKG_REQUIRES_PRIVATE], [$(test x$enable_shared = xno || echo ${pkg_requires})])

# Setup output beautifier.
m4_ifdef([AM_SILENT_RULES], [AM_SILENT_RULES([yes])])

AC_CONFIG_FILES([Makefile libass/Makefile test/Makefile profile/Makefile libass.pc])
AC_OUTPUT
