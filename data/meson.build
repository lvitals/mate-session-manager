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

if get_option('wayland-session')
  i18n.merge_file(
    input: 'mate-wayland.desktop.in',
    output: 'mate-wayland.desktop',
    type: 'desktop',
    po_dir: join_paths(meson.source_root(), 'po'),
    install: true,
    install_dir: join_paths(datadir, 'wayland-sessions')
  )
endif

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
schema_conf = configuration_data()
schema_conf.set('DEFAULT_WM', get_option('default_wm'))
schema_conf.set('GETTEXT_PACKAGE', meson.project_name())
configured_schema = configure_file(
  input: 'org.mate.session.gschema.xml.in',
  output: 'org.mate.session.gschema.xml.in',
  configuration: schema_conf
)

merged_schema = i18n.merge_file(
  input: configured_schema,
  output: 'org.mate.session.gschema.xml',
  po_dir: join_paths(meson.source_root(), 'po'),
  install: true,
  install_dir: join_paths(datadir, 'glib-2.0', 'schemas')
)

# Compile the generated schema during build
glib_compile_schemas = find_program('glib-compile-schemas')
custom_target(
  'gsettings-compile-data',
  input: merged_schema,
  output: 'gschemas.compiled',
  command: [
    glib_compile_schemas,
    '--targetdir', meson.current_build_dir(),
    meson.current_build_dir()
  ],
  build_by_default: true
)

subdir('icons')
