# Install scripts and UI data
install_data('mate-wm',
  install_dir: join_paths(prefix, get_option('bindir')),
  install_mode: 'rwxr-xr-x'
)

install_data(
  'session-properties.ui',
  'gsm-inhibit-dialog.ui',
  'hardware-compatibility',
  install_dir: pkgdatadir
)

# Desktop files
i18n.merge_file(
  input: 'mate.desktop.in',
  output: 'mate.desktop',
  type: 'desktop',
  po_dir: join_paths(meson.source_root(), 'po'),
  install: true,
  install_dir: join_paths(datadir, 'xsessions')
)

i18n.merge_file(
  input: 'mate-session-properties.desktop.in',
  output: 'mate-session-properties.desktop',
  type: 'desktop',
  po_dir: join_paths(meson.source_root(), 'po'),
  install: true,
  install_dir: join_paths(datadir, 'applications')
)

if get_option('default_wm') == 'mate-wm'
  i18n.merge_file(
    input: 'mate-wm.desktop.in',
    output: 'mate-wm.desktop',
    type: 'desktop',
    po_dir: join_paths(meson.source_root(), 'po'),
    install: true,
    install_dir: join_paths(datadir, 'applications')
  )
endif

if get_option('default_wm') == 'marco'
  i18n.merge_file(
    input: 'marco.desktop.in',
    output: 'marco.desktop',
    type: 'desktop',
    po_dir: join_paths(meson.source_root(), 'po'),
    install: true,
    install_dir: join_paths(datadir, 'applications')
  )
endif

# GSettings schemas
i18n.merge_file(
  input: 'org.mate.session.gschema.xml.in',
  output: 'org.mate.session.gschema.xml',
  po_dir: join_paths(meson.source_root(), 'po'),
  install: true,
  install_dir: join_paths(datadir, 'glib-2.0', 'schemas')
)

# Compile schemas during build
gnome.compile_schemas(
  depend_files: files('org.mate.session.gschema.xml.in')
)

subdir('icons')
