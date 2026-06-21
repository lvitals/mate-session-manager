# Glib genmarshal
marshal_files = gnome.genmarshal('gsm-marshal',
  sources: 'gsm-marshal.list',
  prefix: 'gsm_marshal'
)

# DBus Glues using dbus-binding-tool
dbus_binding_tool = find_program('dbus-binding-tool')

dbus_glues = []
glue_targets = [
  ['gsm-manager-glue.h', 'org.gnome.SessionManager.xml', 'gsm_manager'],
  ['gsm-client-glue.h', 'org.gnome.SessionManager.Client.xml', 'gsm_client'],
  ['gsm-inhibitor-glue.h', 'org.gnome.SessionManager.Inhibitor.xml', 'gsm_inhibitor'],
  ['gsm-presence-glue.h', 'org.gnome.SessionManager.Presence.xml', 'gsm_presence']
]

foreach t : glue_targets
  dbus_glues += custom_target(t[0],
    input: t[1],
    output: t[0],
    command: [
      dbus_binding_tool,
      '--prefix=' + t[2],
      '--mode=glib-server',
      '--output=@OUTPUT@',
      '@INPUT@'
    ]
  )
endforeach

# GDBus Codegen
app_codegen = gnome.gdbus_codegen('org.gnome.SessionManager.App',
  sources: 'org.gnome.SessionManager.App.xml',
  interface_prefix: 'org.gnome.SessionManager.App.',
  namespace: 'Gsm',
  extra_args: [
    '--annotate', 'org.gnome.SessionManager.App', 'org.gtk.GDBus.C.Name', 'ExportedApp'
  ]
)

# Static helper library libgsmutil
libgsmutil = static_library('gsmutil',
  'gsm-util.c',
  include_directories: include_dirs,
  dependencies: [glib_dep, gio_dep, gtk_dep, dbus_glib_dep]
)

libgsmutil_dep = declare_dependency(
  link_with: libgsmutil,
  include_directories: include_directories('.')
)

# Build mate-session executable
prefix = get_option('prefix')
datadir = join_paths(prefix, get_option('datadir'))
libexecdir = join_paths(prefix, get_option('libexecdir'))
pkgdatadir = join_paths(datadir, 'mate-session')
localedir = join_paths(prefix, get_option('localedir'))

mate_session_sources = files(
  'gsm-app.c',
  'gsm-autostart-app.c',
  'gsm-client.c',
  'gsm-xsmp-client.c',
  'gsm-dbus-client.c',
  'gsm-consolekit.c',
  'gsm-systemd.c',
  'gsm-logout-dialog.c',
  'gsm-inhibit-dialog.c',
  'gs-idle-monitor.c',
  'gsm-presence.c',
  'mdm.c',
  'mdm-signal-handler.c',
  'mdm-log.c',
  'msm-gnome.c',
  'main.c',
  'gsm-store.c',
  'gsm-inhibitor.c',
  'gsm-manager.c',
  'gsm-session-save.c',
  'gsm-xsmp-server.c',
)

mate_session_c_args = [
  '-DLOCALE_DIR="@0@"'.format(localedir),
  '-DDATA_DIR="@0@"'.format(pkgdatadir),
  '-DLIBEXECDIR="@0@"'.format(libexecdir),
  '-DGTKBUILDER_DIR="@0@"'.format(pkgdatadir),
  '-DI_KNOW_THE_DEVICEKIT_POWER_API_IS_SUBJECT_TO_CHANGE'
]

mate_session_deps = [
  glib_dep, gio_dep, gtk_dep, dbus_glib_dep,
  x11_dep, sm_dep, ice_dep, xext_dep, xau_dep, xcomposite_dep, gl_dep, epoxy_dep,
  gles_dep, egl_dep, libcanberra_dep, systemd_dep, elogind_dep, xtst_dep, xrender_dep, xtrans_dep,
  libgsmutil_dep, libegg_dep
] + execinfo_lib

executable('mate-session',
  mate_session_sources,
  marshal_files,
  app_codegen,
  dbus_glues,
  include_directories: include_dirs,
  c_args: mate_session_c_args,
  dependencies: mate_session_deps,
  install: true
)

executable('test-inhibit', 'test-inhibit.c',
  include_directories: include_dirs,
  dependencies: [glib_dep, gio_dep, gtk_dep, dbus_glib_dep],
  install: false
)

executable('test-client-dbus', 'test-client-dbus.c',
  include_directories: include_dirs,
  dependencies: [glib_dep, gio_dep, gtk_dep, dbus_glib_dep],
  install: false
)
