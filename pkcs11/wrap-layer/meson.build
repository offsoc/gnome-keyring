libgkm_wrap_layer_sources = files(
  'gkm-wrap-layer.c',
  'gkm-wrap-login.c',
  'gkm-wrap-prompt.c',
)

libgkm_wrap_layer_deps = [
  glib_dep,
  gobject_dep,
  gcr_base_dep,
  libgcrypt_dep,

  libegg_dep,
  libgkm_dep,
]

libgkm_wrap_layer = static_library('gkm-wrap-layer-standalone',
  sources: libgkm_wrap_layer_sources,
  dependencies: libgkm_wrap_layer_deps,
  include_directories: config_h_inc,
)

libgkm_wrap_layer_dep = declare_dependency(
  link_with: libgkm_wrap_layer,
)

# Tests
libgkm_wrap_layer_tests = [
  {
    'name': 'test-create-credential',
    'needs-mock': false,
  },
  {
    'name': 'test-init-pin',
    'needs-mock': false,
  },
  {
    'name': 'test-login-auto',
    'needs-mock': true,
  },
  {
    'name': 'test-login-hints',
    'needs-mock': false,
  },
  {
    'name': 'test-login-keyring',
    'needs-mock': true,
  },
  {
    'name': 'test-login-specific',
    'needs-mock': false,
  },
  {
    'name': 'test-login-user',
    'needs-mock': false,
  },
  {
    'name': 'test-set-pin',
    'needs-mock': false,
  },
]

libgkm_wrap_layer_tests_deps = [
  gck_dep,
  gcr_base_dep,

  libgkd_test_dep,
  libgkm_wrap_layer_deps,
  libgkm_wrap_layer_dep,
]

libgkm_wrap_layer_tests_cflags = [
]

foreach _test : libgkm_wrap_layer_tests
  test_name = _test['name']

  test_sources = [ '@0@.c'.format(test_name) ]
  if _test['needs-mock']
    test_sources += 'mock-secret-store.c'
  endif

  test_bin = executable(test_name,
    test_sources,
    dependencies: libgkm_wrap_layer_tests_deps,
    c_args: libgkm_wrap_layer_tests_cflags,
    include_directories: config_h_inc,
  )

  test(test_name, test_bin,
    suite: 'gkm::wrap-layer',
  )
endforeach
