libpam_gkr_sources = files(
  'gkr-pam-client.c',
  'gkr-pam-module.c',
  'gkr-pam-stubs.c',
)

libpam_gkr_deps = [
  libpam_dep,
  libegg_dep,
]

if selinux_dep.found()
  libpam_gkr_deps += selinux_dep
endif

libpam_gkr_cflags = [
  '-DGNOME_KEYRING_DAEMON="@0@"'.format(gkr_prefix / get_option('bindir') / 'gnome-keyring-daemon'),
]

libpam_gkr = shared_library('pam_gnome_keyring',
  libpam_gkr_sources,
  c_args: libpam_gkr_cflags,
  dependencies: libpam_gkr_deps,
  include_directories: config_h_inc,
  install: true,
  install_dir: get_option('libdir') / 'security',
)

libpam_gkr_dep = declare_dependency(
  link_with: libpam_gkr,
)

# Tests
libpam_gkr_test_deps = [
  glib_dep,
  gio_dep,
  libpam_dep,

  libegg_dep,
  libgkd_test_dep,
  libgkd_control_dep,
]

libpam_gkr_test_cflags = [
  '-DSRCDIR="@0@"'.format(source_root),
  '-DBUILDDIR="@0@"'.format(build_root),
  '-DSYSCONFDIR="@0@"'.format(get_option('sysconfdir')),
]

libpam_gkr_test_bin = executable('test-pam',
  'test-pam.c',
  dependencies: libpam_gkr_test_deps,
  c_args: libpam_gkr_test_cflags,
  include_directories: config_h_inc,
)

test('test-pam', libpam_gkr_test_bin,
  suite: 'pam',
)
