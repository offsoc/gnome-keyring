libgkm_gnome2_store_sources = files(
  'gkm-gnome2-file.c',
  'gkm-gnome2-module.c',
  'gkm-gnome2-private-key.c',
  'gkm-gnome2-public-key.c',
  'gkm-gnome2-storage.c',
)

libgkm_gnome2_store_deps = [
  glib_dep,
  gobject_dep,
  libgcrypt_dep,

  libegg_dep,
  libgkm_dep,
]

libgkm_gnome2_store = library('gkm-gnome2-store-standalone',
  libgkm_gnome2_store_sources,
  name_prefix: '',
  dependencies: libgkm_gnome2_store_deps,
  include_directories: config_h_inc,
  install: true,
  install_dir: gkr_pkglibdir,
)

libgkm_gnome2_store_dep = declare_dependency(
  link_with: libgkm_gnome2_store,
)

# Tests
libgkm_gnome2_store_tests = [
  {
    'name': 'test-gnome2-file',
    'needs-mock': false,
  },
  {
    'name': 'test-gnome2-storage',
    'needs-mock': true,
  },
  {
    'name': 'test-gnome2-private-key',
    'needs-mock': true,
  },
  {
    'name': 'test-import',
    'needs-mock': false,
  },
]

libgkm_gnome2_store_tests_deps = [
  gck_dep,
  gcr_base_dep,

  libgkm_gnome2_store_deps,
  libgkm_gnome2_store_dep,
]

libgkm_gnome2_store_tests_cflags = [
  '-DSRCDIR="@0@"'.format(source_root),
]

foreach _test : libgkm_gnome2_store_tests
  test_name = _test['name']

  test_sources = [ '@0@.c'.format(test_name) ]
  if _test['needs-mock']
    test_sources += 'mock-gnome2-module.c'
  endif

  test_bin = executable(test_name,
    test_sources,
    dependencies: libgkm_gnome2_store_tests_deps,
    c_args: libgkm_gnome2_store_tests_cflags,
    include_directories: config_h_inc,
  )

  test(test_name, test_bin,
    suite: 'gkm::gnome2-store',
  )
endforeach
