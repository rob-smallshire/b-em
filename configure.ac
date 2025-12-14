# configure.ac for B-em

AC_INIT([B-em],[m4_esyscmd_s(utils/b-em-version-str.sh)],[thomas@xteddy.org])

AC_CONFIG_AUX_DIR(etc)
AM_INIT_AUTOMAKE([-Wall -Werror foreign subdir-objects])
AM_SILENT_RULES([yes])

AC_CANONICAL_HOST

# Set up the compiler in two different ways and say yes we may want to install.
AC_PROG_CC
AC_PROG_CXX
CXXFLAGS="$CXXFLAGS -std=gnu++11"
AM_PROG_CC_C_O

# Whether this is a released build or not.
ISRELEASED="no"

AC_MSG_CHECKING([whether to enable debugging])
AC_ARG_ENABLE(debug,
	      AS_HELP_STRING([--enable-debug],[build debug executable]))

CF_WARNINGS="-Wall -Wno-format-security"
CFLAGS="$CFLAGS $CF_WARNINGS -std=gnu11 -D_GNU_SOURCE"
if test "$enable_debug" = "yes"; then
   CFLAGS="$CFLAGS -O3 -ggdb -D_DEBUG"
   LDFLAGS="$LDFLAGS -L/usr/local/lib"
   AC_MSG_RESULT([yes])
else
   CFLAGS="$CFLAGS -O3"
   LDFLAGS="$LDFLAGS -L/usr/local/lib"
   AC_MSG_RESULT([no])
fi

# Checks for libraries.
PKG_CHECK_MODULES([ALLEGRO], [allegro-5 allegro_acodec-5 allegro_audio-5 allegro_dialog-5 allegro_image-5 allegro_primitives-5 allegro_font-5 allegro_main-5])
AC_CHECK_LIB([m], [sin])

# Checks for header files.
AC_CHECK_HEADERS([inttypes.h limits.h malloc.h stddef.h stdint.h stdlib.h string.h unistd.h])

AM_CONDITIONAL(OS_WIN, test "$host_os" = "win")

# Checks for typedefs, structures, and compiler characteristics.
AC_CHECK_HEADER_STDBOOL
AC_C_INLINE
AC_TYPE_INT16_T
AC_TYPE_INT32_T
AC_TYPE_INT64_T
AC_TYPE_INT8_T
AC_TYPE_OFF_T
AC_TYPE_SIZE_T
AC_TYPE_UINT16_T
AC_TYPE_UINT32_T
AC_TYPE_UINT64_T
AC_TYPE_UINT8_T

# DB: check if max_align_t is defined
AC_CHECK_TYPES([max_align_t])


# Checks for library functions.
AC_FUNC_ALLOCA
AC_FUNC_ERROR_AT_LINE
AC_FUNC_MALLOC
AC_FUNC_MKTIME
AC_CHECK_FUNCS([asprintf atexit floor memset mkdir pow rmdir sqrt stpcpy strcasecmp strchr strdup strerror strncasecmp strrchr strtol tdestroy])

# Check tsearch for tdestroy and include that for non-GNU systems.
AC_CHECK_FUNC(tdestroy, found_tdestroy=yes, found_tdestroy=no)
if test "x$found_tdestroy" = xyes; then
	AC_DEFINE(HAVE_TDESTROY)
fi

AM_CONDITIONAL(NO_TSEARCH, [test x"$found_tdestroy" = xno ])

# Checks for Linux MIDI implementations.
AC_CHECK_HEADERS(jack/jack.h alsa/asoundlib.h)
AC_SEARCH_LIBS(jack_port_register,jack jack2)
AC_CHECK_LIB(asound,snd_rawmidi_open)

AC_CONFIG_FILES([Makefile src/Makefile])
AC_OUTPUT

echo
echo "Configured version: $VERSION"
echo
