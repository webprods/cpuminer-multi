
if WANT_JANSSON
JANSSON_INCLUDES= -I$(top_srcdir)/compat/jansson
else
JANSSON_INCLUDES=
endif

EXTRA_DIST	= example-cfg.json nomacro.pl

SUBDIRS		= compat

ALL_INCLUDES	= @PTHREAD_FLAGS@ -fno-strict-aliasing $(JANSSON_INCLUDES)

bin_PROGRAMS	= cpuminer

dist_man_MANS	= cpuminer.1

cpuminer_SOURCES = \
  cpu-miner.c util.c \
  api.c sensors.c \
  sha3/sph_keccak.c \
  sha3/sph_hefty1.c \
  sha3/sph_groestl.c \
  sha3/sph_skein.c \
  sha3/sph_bmw.c \
  sha3/sph_jh.c \
  sha3/sph_shavite.c \
  sha3/sph_blake.c \
  sha3/sph_luffa.c \
  sha3/sph_cubehash.c \
  sha3/sph_simd.c \
  sha3/sph_echo.c \
  sha3/sph_hamsi.c \
  sha3/sph_fugue.c \
  sha3/sph_shabal.c \
  sha3/sph_whirlpool.c \
  crypto/blake2s \
  crypto/oaes_lib.c \
  crypto/c_keccak.c \
  crypto/c_groestl.c \
  crypto/c_blake256.c \
  crypto/c_jh.c \
  crypto/c_skein.c \
  crypto/hash.c \
  crypto/aesb.c \
  lyra2/Lyra2.c lyra2/Sponge.c \
  blake.c \
  cryptonight.c \
  fresh.c \
  heavy.c \
  ink.c \
  lyra2re.c \
  keccak.c \
  pentablake.c \
  quark.c \
  neoscrypt.c \
  nist5.c \
  qubit.c \
  scrypt.c sha2.c \
  skein.c \
  s3.c \
  x11.c \
  x13.c \
  x14.c \
  x15.c

disable_flags =

if USE_ASM
   cpuminer_SOURCES += neoscrypt_asm.S
if ARCH_x86
   cpuminer_SOURCES += sha2-x86.S scrypt-x86.S aesb-x86.S crypto/aesb-x86-impl.c
endif
if ARCH_x86_64
   cpuminer_SOURCES += sha2-x64.S scrypt-x64.S aesb-x64.S
endif
if ARCH_ARM
   cpuminer_SOURCES += sha2-arm.S scrypt-arm.S
endif
else
   disable_flags += -DNOASM
   cpuminer_SOURCES += crypto/aesb-x86-impl.c
endif

if HAVE_WINDOWS
   cpuminer_SOURCES += compat/winansi.c
endif

cpuminer_LDFLAGS	= @LDFLAGS@
cpuminer_LDADD	= @LIBCURL@ @JANSSON_LIBS@ @PTHREAD_LIBS@ @WS2_LIBS@
cpuminer_CPPFLAGS = @LIBCURL_CPPFLAGS@ $(ALL_INCLUDES)
cpuminer_CFLAGS   = -Wno-pointer-sign -Wno-pointer-to-int-cast $(disable_flags)

if HAVE_WINDOWS
cpuminer_CFLAGS += -Wl,--stack,10485760
endif

if HAVE_WINDOWS
# use to profile an object
# gprof_cflags = -pg -g3
# cpuminer_LDFLAGS += -pg
# cpuminer_CFLAGS += -fno-inline-functions -static

# copy/paste from generated Makefile
common_ccflags = $(DEFS) $(ALL_INCLUDES) $(cpuminer_CPPFLAGS) $(CPPFLAGS) $(cpuminer_CFLAGS) $(CFLAGS)

# special CFLAGS (if you find a simpler way to do that tell me ;)
cpuminer-neoscrypt.o: neoscrypt.c
	@echo "CUSTOM ${@}: ${filter %.o,${^}} ${filter %.c,${^}}"
	$(CC) $(common_ccflags) -g -O3 $(gprof_cflags) -MT $@ -MD -MP -c -o $@ $<


endif
