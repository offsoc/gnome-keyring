project('gnome-keyring', 'c',
  version: '42.0',
  meson_version: '>= 0.61',
)

gnome = import('gnome')
i18n = import('i18n')

# Version
gkr_major_version = '3'

# Common variables
cc = meson.get_compiler('c')
gkr_prefix = get_option('prefix')
gkr_bindir = gkr_prefix / get_option('bindir')
gkr_pkglibdir = gkr_prefix / get_option('libdir') / 'gnome-keyring' / 'devel'
config_h_inc = include_directories('.')
po_dir = meson.current_source_dir() / 'po'
source_root = meson.current_source_dir()
build_root = meson.current_build_dir()

# Dependencies
glib_dep = dependency('glib-2.0', version: '>= 2.44')
gio_dep = dependency('gio-2.0')
gio_unix_dep = dependency('gio-unix-2.0')
gmodule_dep = dependency('gmodule-no-export-2.0')
gobject_dep = dependency('gobject-2.0')
threads_dep = dependency('threads')
gck_dep = dependency('gck-1', version: '>= 3.3.4')
gcr_dep = dependency('gcr-3', version: '>= 3.27.90')
gcr_base_dep = dependency('gcr-base-3', version: '>= 3.27.90')
libgcrypt_dep = dependency('libgcrypt', version: '>= 1.2.2')
libcap_ng_dep = dependency('libcap-ng', required: false)
p11_kit_dep = dependency('p11-kit-1')

selinux_dep = dependency('libselinux', required: get_option('selinux'))

if get_option('ssh-agent')
  ssh_agent_bin = find_program('ssh-agent')
  ssh_add_bin = find_program('ssh-add')
endif

if get_option('pam')
  libpam_dep = cc.find_library('pam')
endif

# Project-wide defines
add_project_arguments([
  '-D_GNU_SOURCE',
  '-DGCK_API_SUBJECT_TO_CHANGE',
  '-DGCR_API_SUBJECT_TO_CHANGE',
], language: 'c')

# Configuration
conf = configuration_data()

conf.set_quoted('GETTEXT_PACKAGE', meson.project_name())
conf.set_quoted('LOCALE_DIR', gkr_prefix / get_option('localedir'))
conf.set_quoted('VERSION', meson.project_version())
conf.set_quoted('LIBGCRYPT_VERSION', libgcrypt_dep.version())
conf.set('HAVE_LIBCAPNG', libcap_ng_dep.found())
conf.set('WITH_SELINUX', selinux_dep.found())
if get_option('ssh-agent')
  conf.set('WITH_SSH', true)
  conf.set_quoted('SSH_AGENT', ssh_agent_bin.full_path())
  conf.set_quoted('SSH_ADD', ssh_add_bin.full_path())
endif
conf.set('DOTLOCK_USE_PTHREAD', true)
conf.set('DOTLOCK_GLIB_LOGGING', true)
conf.set('DOTLOCK_EXT_SYM_PREFIX', '_gkm_')
conf.set('HAVE_SOCKLEN_T', true)
conf.set('HAVE_GETPEERUCRED', cc.has_function('getpeerucred'))
conf.set('HAVE_GETPEEREID', cc.has_function('getpeereid'))
conf.set('HAVE_FLOCK', cc.has_function('flock'))
conf.set('HAVE_MLOCK', cc.has_function('mlock'))
conf.set('HAVE_TIMEGM', cc.has_function('timegm'))

configure_file(output: 'config.h', configuration: conf)
config_h_dir = include_directories('.')

# Build libegg (helper module)
subdir('egg')

# Daemon launch lib (needed for tests)
libgkd_test = library('gkd-test',
  files('daemon/gkd-test.c'),
  dependencies: [glib_dep, libegg_dep ],
  # BUILDDIR here refers to the location of the built gnome-keyring-daemon
  c_args: [ '-DBUILDDIR="@0@"'.format(build_root / 'daemon') ],
)

libgkd_test_dep = declare_dependency(
  link_with: libgkd_test,
)

# Other subdirectories
subdir('po')
subdir('pkcs11')
subdir('daemon')
subdir('schema')
subdir('tool')
if get_option('pam')
  subdir('pam')
endif
subdir('docs')

gnome.post_install(
  glib_compile_schemas: true,
)
