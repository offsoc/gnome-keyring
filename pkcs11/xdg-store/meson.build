libgkm_xdg_store_sources = files(
  'gkm-xdg-asn1-defs.c',
  'gkm-xdg-assertion.c',
  'gkm-xdg-module.c',
  'gkm-xdg-trust.c',
)

libgkm_xdg_store_deps = [
  glib_dep,
  gobject_dep,
  libgcrypt_dep,

  libegg_dep,
  libgkm_dep,
]

libgkm_xdg_store = library('gkm-xdg-store',
  libgkm_xdg_store_sources,
  dependencies: libgkm_xdg_store_deps,
  include_directories: config_h_inc,
)

libgkm_xdg_store_dep = declare_dependency(
  link_with: libgkm_xdg_store,
  include_directories: include_directories('..'),
)

# Tests
libgkm_xdg_store_tests = [
  {
    'name': 'test-xdg-module',
    'needs-mock': true,
  },
  {
    'name': 'test-xdg-trust',
    'needs-mock': true,
  },
]

libgkm_xdg_store_tests_deps = [
  gck_dep,
  gcr_base_dep,

  libgkm_xdg_store_deps,
  libgkm_xdg_store_dep,
]

libgkm_xdg_store_tests_cflags = [
  '-DSRCDIR="@0@"'.format(source_root),
]

foreach _test : libgkm_xdg_store_tests
  test_name = _test['name']

  test_sources = [ '@0@.c'.format(test_name) ]
  if _test['needs-mock']
    test_sources += 'mock-xdg-module.c'
  endif

  test_bin = executable(test_name,
    test_sources,
    dependencies: libgkm_xdg_store_tests_deps,
    c_args: libgkm_xdg_store_tests_cflags,
    include_directories: config_h_inc,
  )

  test(test_name, test_bin,
    suite: 'gkm::xdg-store',
  )
endforeach

# Example programs
frob_trust_file = executable('frob-trust-file',
  'frob-trust-file.c',
  dependencies: [ glib_dep, libegg_dep, libgkm_xdg_store_dep ],
  include_directories: config_h_inc,
)
dump_trust_file = executable('dump-trust-file',
  'dump-trust-file.c',
  dependencies: [ glib_dep, libegg_dep, libgkm_xdg_store_dep ],
  include_directories: config_h_inc,
)
