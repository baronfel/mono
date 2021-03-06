if HOST_WIN32
win32_sources = \
	console-win32.c \
	console-win32-internals.h \
	cominterop-win32-internals.h \
	w32file-win32.c \
	w32file-win32-internals.h \
	icall-windows.c \
	icall-windows-internals.h \
	marshal-windows.c \
	marshal-windows-internals.h \
	mono-security-windows.c \
	mono-security-windows-internals.h \
	w32mutex-win32.c \
	w32semaphore-win32.c \
	w32event-win32.c \
	w32process-win32.c \
	w32process-win32-internals.h \
	w32socket-win32.c \
	w32error-win32.c

platform_sources = $(win32_sources)

# Use -m here. This will use / as directory separator (C:/WINNT).
# The files that use MONO_ASSEMBLIES and/or MONO_CFG_DIR replace the
# / by \ if running under WIN32.
if CROSS_COMPILING
assembliesdir = ${libdir}
confdir = ${sysconfdir}
else
assembliesdir = `cygpath -m "${libdir}"`
confdir = `cygpath -m "${sysconfdir}"`
endif
export HOST_CC
# The mingw math.h has "extern inline" functions that dont appear in libs, so
# optimisation is required to actually inline them
AM_CFLAGS = -O
else

assembliesdir = $(exec_prefix)/lib
confdir = $(sysconfdir)
unix_sources = \
	console-unix.c \
	w32mutex-unix.c \
	w32semaphore-unix.c \
	w32event-unix.c \
	w32process-unix.c \
	w32process-unix-internals.h \
	w32process-unix-osx.c \
	w32process-unix-bsd.c \
	w32process-unix-haiku.c \
	w32process-unix-default.c \
	w32socket-unix.c \
	w32file-unix.c \
	w32file-unix-glob.c \
	w32file-unix-glob.h \
	w32error-unix.c

platform_sources = $(unix_sources)
endif

if HOST_ANDROID
platform_sources += ../../support/libm/complex.c
endif

if HOST_WASM
platform_sources += threadpool-worker-wasm.c
else
platform_sources += threadpool-worker-default.c
endif

if BITCODE
libmonoldflags = -no-undefined
endif

if SUPPORT_SGEN
sgen_libraries = libmonoruntimesgen.la
endif

if SUPPORT_BOEHM
boehm_libraries = libmonoruntime.la
endif

if DISABLE_ICALL_TABLES
icall_table_libraries = libmono-icall-table.la
endif

noinst_LTLIBRARIES = libmonoruntime-config.la $(boehm_libraries) $(sgen_libraries)

lib_LTLIBRARIES = $(icall_table_libraries)

AM_CPPFLAGS = -I$(top_srcdir) -I$(top_srcdir)/mono $(LIBGC_CPPFLAGS) $(GLIB_CFLAGS) $(SHARED_CFLAGS)

#
# Make sure any prefix changes are updated in the binaries too.
#
# This won't result in many more false positives than AC_DEFINEing them
# in configure.ac.
#
mono-config-dirs.lo: Makefile

#
# This library is used to localize the usage of MONO_BINDIR etc. to just one source file, thus enabling
# ccache to work even if the value of these defines change. We need to use a convenience library since automake
# doesn't support per file cflags.
#
libmonoruntime_config_la_SOURCES = \
	mono-config-dirs.h		\
	mono-config-dirs.c
libmonoruntime_config_la_CPPFLAGS = $(AM_CPPFLAGS) -DMONO_BINDIR=\"$(bindir)/\" -DMONO_ASSEMBLIES=\"$(assembliesdir)\" -DMONO_CFG_DIR=\"$(confdir)\" -DMONO_RELOC_LIBDIR=\"../$(reloc_libdir)\"

#
# This library contains the icall tables if the runtime was configured with --disable-icall-tables
#
if DISABLE_ICALL_TABLES
libmono_icall_table_la_SOURCES = \
	icall-table.c
libmono_icall_table_la_CFLAGS = $(SGEN_DEFINES)
libmono_icall_table_la_LDFLAGS = $(libmonoldflags)
if BITCODE
libmono_icall_table_la_LIBADD = ../eglib/libeglib.la ../utils/libmonoutils.la ../sgen/libmonosgen.la libmonoruntimesgen.la
endif
endif

CLEANFILES = mono-bundle.stamp

null_sources = \
	console-null.c

null_gc_sources = \
	null-gc.c \
	null-gc-handles.h \
	null-gc-handles.c

if !DISABLE_ICALL_TABLES
icall_tables_sources = \
	icall-table.c
endif

common_sources = \
	$(platform_sources)	\
	appdomain.c	\
	domain.c	\
	appdomain-icalls.h	\
	assembly.c		\
	assembly-internals.h	\
	attach.h		\
	attach.c		\
	cil-coff.h		\
	class.c			\
	class-getters.h		\
	class-init.h		\
	class-init.c		\
	class-internals.h	\
	class-inlines.h		\
	class-private-definition.h	\
	class-accessors.c	\
	cominterop.c		\
	cominterop.h		\
	console-io.h		\
	coree.c 		\
	coree.h 		\
	coree-internals.h \
	culture-info.h  	\
	culture-info-tables.h	\
	debug-helpers.c		\
	debug-mono-symfile.h	\
	debug-mono-symfile.c	\
	debug-mono-ppdb.h	\
	debug-mono-ppdb.c	\
	decimal-ms.c		\
	decimal-ms.h		\
	domain-internals.h	\
	environment.c		\
	environment.h		\
	exception.c		\
	exception.h		\
	exception-internals.h	\
	w32file.c		\
	w32file.h		\
	w32file-internals.h \
	filewatcher.c		\
	filewatcher.h		\
	gc-internals.h		\
	icall.c			\
	icall-internals.h \
	icall-def.h		\
	icall-table.h	\
	image.c			\
	image-internals.h	\
	jit-info.c		\
	loader.c		\
	locales.c		\
	locales.h		\
	lock-tracer.c		\
	lock-tracer.h		\
	marshal.c		\
	marshal.h		\
	marshal-internals.h \
	mempool.c		\
	mempool.h		\
	mempool-internals.h	\
	metadata.c		\
	metadata-verify.c	\
	metadata-internals.h	\
	method-builder.h 	\
	method-builder.c 	\
	mono-basic-block.c	\
	mono-basic-block.h	\
	mono-config.c		\
	mono-debug.h		\
	mono-debug.c		\
	debug-internals.h	\
	mono-endian.c		\
	mono-endian.h		\
	mono-hash.c		\
	mono-hash.h		\
	mono-conc-hash.c		\
	mono-conc-hash.h		\
	mono-mlist.c		\
	mono-mlist.h		\
	mono-perfcounters.c	\
	mono-perfcounters.h	\
	mono-perfcounters-def.h	\
	mono-ptr-array.h	\
	mono-route.c		\
	mono-route.h		\
	monitor.h		\
	normalization-tables.h	\
	number-formatter.h	\
	number-ms.c		\
	number-ms.h		\
	object.c	\
	object-forward.h	\
	object-internals.h	\
	opcodes.c		\
	property-bag.h	\
	property-bag.c	\
	w32socket.c		\
	w32socket.h		\
	w32socket-internals.h		\
	w32process.c		\
	w32process.h		\
	w32process-internals.h		\
	profiler.c		\
	profiler-private.h	\
	rand.h			\
	rand.c			\
	remoting.h		\
	remoting.c		\
	runtime.c		\
	mono-security.c		\
	security.h		\
	security-core-clr.c	\
	security-core-clr.h	\
	security-manager.c	\
	security-manager.h	\
	string-icalls.c 	\
	string-icalls.h 	\
	sysmath.h		\
	sysmath.c		\
	tabledefs.h 		\
	threads.c		\
	threads-types.h		\
	threadpool.c	\
	threadpool.h	\
	threadpool-worker.h	\
	threadpool-io.c	\
	threadpool-io.h	\
	verify.c		\
	verify-internals.h	\
	wrapper-types.h	\
	dynamic-image-internals.h	\
	dynamic-stream.c	\
	dynamic-stream-internals.h	\
	reflection-cache.h	\
	custom-attrs-internals.h	\
	sre-internals.h	\
	reflection-internals.h	\
	file-mmap-posix.c	\
	file-mmap-windows.c	\
	file-mmap.h	\
	object-offsets.h	\
	abi-details.h	\
	class-abi-details.h	\
	metadata-cross-helpers.c	\
	seq-points-data.h	\
	seq-points-data.c	\
	handle.c	\
	handle.h	\
	w32mutex.h	\
	w32semaphore.h	\
	w32event.h	\
	w32handle-namespace.h	\
	w32handle-namespace.c	\
	w32handle.h	\
	w32handle.c	\
	w32error.h	\
	reflection.c	\
	dynamic-image.c	\
	sre.c	\
	sre-encode.c	\
	sre-save.c	\
	custom-attrs.c	\
	fdhandle.h	\
	fdhandle.c	\
	callspec.h	\
	callspec.c

# These source files have compile time dependencies on GC code
gc_dependent_sources = \
	gc-stats.c	\
	gc.c		\
	monitor.c

boehm_sources = \
	boehm-gc.c

sgen_sources = \
	sgen-bridge.c		\
	sgen-bridge.h		\
	sgen-bridge-internals.h	\
	sgen-old-bridge.c		\
	sgen-new-bridge.c		\
	sgen-tarjan-bridge.c		\
	sgen-toggleref.c		\
	sgen-toggleref.h		\
	sgen-stw.c				\
	sgen-mono.c		\
	sgen-client-mono.h

libmonoruntime_la_SOURCES = $(common_sources) $(icall_tables_sources) $(gc_dependent_sources) $(null_gc_sources) $(boehm_sources)
libmonoruntime_la_CFLAGS = $(BOEHM_DEFINES)
libmonoruntime_la_LIBADD = libmonoruntime-config.la

libmonoruntimesgen_la_SOURCES = $(common_sources) $(icall_tables_sources) $(gc_dependent_sources) $(sgen_sources)
libmonoruntimesgen_la_CFLAGS = $(SGEN_DEFINES)
libmonoruntimesgen_la_LIBADD = libmonoruntime-config.la

libmonoruntimeincludedir = $(includedir)/mono-$(API_VER)/mono/metadata

# This list is sorted for easier searching.
libmonoruntimeinclude_HEADERS = \
	appdomain.h		\
	assembly.h		\
	attrdefs.h		\
	blob.h			\
	class.h			\
	debug-helpers.h		\
	debug-mono-symfile.h	\
	environment.h		\
	exception.h		\
	image.h			\
	loader.h		\
	metadata.h		\
	mono-config.h		\
	mono-debug.h		\
	mono-gc.h		\
	object.h		\
	object-forward.h	\
	opcodes.h		\
	profiler.h		\
	profiler-events.h	\
	reflection.h		\
	row-indexes.h		\
	sgen-bridge.h		\
	threads.h		\
	tokentype.h		\
	verify.h		

EXTRA_DIST = $(win32_sources) $(unix_sources) $(null_sources) runtime.h \
		threadpool-io-poll.c threadpool-io-epoll.c threadpool-io-kqueue.c sgen-dynarray.h
