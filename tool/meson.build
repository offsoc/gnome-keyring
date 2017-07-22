gnome_keyring_tool_sources = files(
  'gkr-tool.c',
  'gkr-tool-import.c',
  'gkr-tool-trust.c',
  'gkr-tool-version.c',
)

gnome_keyring_tool_deps = [
  glib_dep,
  gck_dep,
  gcr_base_dep,
  gcr_ui_dep,
  libegg_dep,
]

gnome_keyring_tool = executable('gnome-keyring-@0@'.format(gkr_major_version),
  gnome_keyring_tool_sources,
  dependencies: gnome_keyring_tool_deps,
  include_directories: config_h_inc,
)
