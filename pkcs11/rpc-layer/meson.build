libgkm_rpc_layer_sources = files(
  'gkm-rpc-dispatch.c',
  'gkm-rpc-message.c',
  'gkm-rpc-util.c',
)

libgkm_rpc_layer_deps = [
  glib_dep,
  gobject_dep,
  libgcrypt_dep,

  libegg_dep,
  libgkm_dep,
]

libgkm_rpc_layer = library('gkm-rpc-layer',
  libgkm_rpc_layer_sources,
  dependencies: libgkm_rpc_layer_deps,
  include_directories: config_h_inc,
  install: true,
  install_dir: gkr_pkglibdir,
)

libgkm_rpc_layer_dep = declare_dependency(
  link_with: libgkm_rpc_layer,
)

# This is the configuration file that p11-kit uses to load the module
p11_system_config_module = get_option('pkcs11-config')
if p11_system_config_module == ''
  p11_system_config_module = p11_kit_dep.get_pkgconfig_variable('p11_system_config_modules')
endif
configure_file(
  input: 'gnome-keyring.module.in',
  output: '@BASENAME@',
  copy: true,
  install_dir: p11_system_config_module,
)

# Tests
libgkm_rpc_layer_tests = [
  'test-initialize',
]

libgkm_rpc_layer_tests_deps = [
  gck_dep,
  gcr_base_dep,

  libgkd_test_dep,
  libgkm_rpc_layer_deps,
  libgkm_rpc_layer_dep,
]

libgkm_rpc_layer_tests_cflags = [
  '-DSRCDIR="@0@"'.format(source_root),
  '-DBUILDDIR="@0@"'.format(meson.current_build_dir()),
]

foreach test_name : libgkm_rpc_layer_tests
  test_bin = executable(test_name,
    '@0@.c'.format(test_name),
    dependencies: libgkm_rpc_layer_tests_deps,
    c_args: libgkm_rpc_layer_tests_cflags,
    include_directories: config_h_inc,
  )

  test(test_name, test_bin,
    suite: 'gkm::rpc-layer',
  )
endforeach
