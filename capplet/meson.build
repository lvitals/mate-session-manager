capplet_sources = files(
  'main.c',
  'gsm-properties-dialog.c',
  'gsm-app-dialog.c',
  'msm-desktop-app-dialog.c',
  'gsp-app.c',
  'gsp-app-manager.c',
  'gsp-keyfile.c'
)

capplet_c_args = [
  '-DLOCALE_DIR="@0@"'.format(localedir),
  '-DGTKBUILDER_DIR="@0@"'.format(pkgdatadir),
]

executable('mate-session-properties',
  capplet_sources,
  include_directories: include_dirs,
  c_args: capplet_c_args,
  dependencies: [glib_dep, gio_dep, gtk_dep, libgsmutil_dep],
  install: true
)
