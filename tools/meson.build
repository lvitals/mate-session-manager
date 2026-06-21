tools_c_args = [
  '-DLOCALE_DIR="@0@"'.format(localedir),
]

executable('mate-session-save', 'mate-session-save.c',
  include_directories: include_dirs,
  c_args: tools_c_args,
  dependencies: [glib_dep, gio_dep, gtk_dep, dbus_glib_dep, sm_dep, ice_dep],
  install: true
)

executable('mate-session-inhibit', 'mate-session-inhibit.c',
  include_directories: include_dirs,
  c_args: tools_c_args,
  dependencies: [glib_dep, gio_dep, gtk_dep, dbus_glib_dep],
  install: true
)

executable('mate-session-check-accelerated', 'mate-session-check-accelerated.c',
  include_directories: include_dirs,
  c_args: tools_c_args + ['-DLIBEXECDIR="@0@"'.format(libexecdir)],
  dependencies: [gtk_dep, gl_dep, epoxy_dep, x11_dep, xcomposite_dep],
  install: true,
  install_dir: libexecdir
)

executable('mate-session-check-accelerated-gl-helper', 'mate-session-check-accelerated-gl-helper.c',
  include_directories: include_dirs,
  c_args: tools_c_args + ['-DPKGDATADIR="@0@"'.format(pkgdatadir)],
  dependencies: [glib_dep, gl_dep, epoxy_dep, x11_dep, xcomposite_dep],
  install: true,
  install_dir: libexecdir
)

if get_option('glesv2') and gles_dep.found() and egl_dep.found()
  executable('mate-session-check-accelerated-gles-helper', 'mate-session-check-accelerated-gles-helper.c',
    include_directories: include_dirs,
    c_args: tools_c_args + ['-DPKGDATADIR="@0@"'.format(pkgdatadir)],
    dependencies: [gles_dep, egl_dep, gtk_dep, x11_dep],
    install: true,
    install_dir: libexecdir
  )
endif
