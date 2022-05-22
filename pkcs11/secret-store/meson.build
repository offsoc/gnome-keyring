libgkm_secret_store_sources = files(
  'gkm-secret-binary.c',
  'gkm-secret-collection.c',
  'gkm-secret-compat.c',
  'gkm-secret-data.c',
  'gkm-secret-fields.c',
  'gkm-secret-item.c',
  'gkm-secret-module.c',
  'gkm-secret-object.c',
  'gkm-secret-search.c',
  'gkm-secret-textual.c',
)

libgkm_secret_store_deps = [
  glib_dep,
  gobject_dep,
  libgcrypt_dep,

  libegg_dep,
  libgkm_dep,
]

libgkm_secret_store = library('gkm-secret-store-standalone',
  libgkm_secret_store_sources,
  name_prefix: '',
  dependencies: libgkm_secret_store_deps,
  include_directories: config_h_inc,
  install: true,
  install_dir: gkr_pkglibdir,
)

libgkm_secret_store_dep = declare_dependency(
  link_with: libgkm_secret_store,
)

# Tests
dump_keyring0_format = executable('dump-keyring0-format',
  'dump-keyring0-format.c',
  dependencies: libgkm_secret_store_deps,
)

libgkm_secret_store_tests = [
  {
    'name': 'test-secret-compat',
    'needs-mock': false,
  },
  {
    'name': 'test-secret-fields',
    'needs-mock': false,
  },
  {
    'name': 'test-secret-data',
    'needs-mock': false,
  },
  {
    'name': 'test-secret-object',
    'needs-mock': true,
  },
  {
    'name': 'test-secret-collection',
    'needs-mock': true,
  },
  {
    'name': 'test-secret-item',
    'needs-mock': true,
  },
  {
    'name': 'test-secret-schema',
    'needs-mock': true,
  },
  {
    'name': 'test-secret-search',
    'needs-mock': true,
  },
  {
    'name': 'test-secret-textual',
    'needs-mock': true,
  },
  {
    'name': 'test-secret-binary',
    'needs-mock': true,
  },
]

libgkm_secret_store_tests_deps = [
  gck_dep,
  gcr_base_dep,

  libgkm_secret_store_deps,
  libgkm_secret_store_dep,
]

libgkm_secret_store_tests_cflags = [
  '-DSRCDIR="@0@"'.format(source_root),
]

foreach _test : libgkm_secret_store_tests
  test_name = _test['name']

  test_sources = [ '@0@.c'.format(test_name) ]
  if _test['needs-mock']
    test_sources += 'mock-secret-module.c'
  endif

  test_bin = executable(test_name,
    test_sources,
    dependencies: libgkm_secret_store_tests_deps,
    c_args: libgkm_secret_store_tests_cflags,
    include_directories: config_h_inc,
  )

  test(test_name, test_bin,
    suite: 'gkm::secret-store',
  )
endforeach
