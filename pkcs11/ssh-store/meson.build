libgkm_ssh_store_sources = files(
  'gkm-ssh-module.c',
  'gkm-ssh-openssh.c',
  'gkm-ssh-private-key.c',
  'gkm-ssh-public-key.c',
)

libgkm_ssh_store_deps = [
  glib_dep,
  gobject_dep,
  libgcrypt_dep,

  libegg_dep,
  libgkm_dep,
]

libgkm_ssh_store = library('gkm-ssh-store-standalone',
  libgkm_ssh_store_sources,
  name_prefix: '',
  dependencies: libgkm_ssh_store_deps,
  include_directories: config_h_inc,
  install: true,
  install_dir: gkr_pkglibdir,
)

libgkm_ssh_store_dep = declare_dependency(
  link_with: libgkm_ssh_store,
)

# Tests
libgkm_ssh_store_tests = [
  {
    'name': 'test-ssh-openssh',
    'needs-mock': false,
  },
  {
    'name': 'test-private-key',
    'needs-mock': true,
  },
]

libgkm_ssh_store_tests_deps = [
  gck_dep,
  gcr_base_dep,

  libgkm_ssh_store_deps,
  libgkm_ssh_store_dep,
]

libgkm_ssh_store_tests_cflags = [
  '-DSRCDIR="@0@"'.format(source_root),
]

foreach _test : libgkm_ssh_store_tests
  test_name = _test['name']

  test_sources = [ '@0@.c'.format(test_name) ]
  if _test['needs-mock']
    test_sources += 'mock-ssh-module.c'
  endif

  test_bin = executable(test_name,
    test_sources,
    dependencies: libgkm_ssh_store_tests_deps,
    c_args: libgkm_ssh_store_tests_cflags,
    include_directories: config_h_inc,
  )

  test(test_name, test_bin,
    suite: 'gkm::ssh-store',
  )
endforeach
