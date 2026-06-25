project('mate-session-manager', 'c',
  version: '1.28.0',
  meson_version: '>= 0.50.0',
  default_options: [
    'warning_level=1',
    'c_std=gnu99',
  ]
)

gnome = import('gnome')
i18n = import('i18n')

cc = meson.get_compiler('c')

# Packages
glib_req = '>= 2.50.0'
gio_req = '>= 2.25.0'
gtk_req = '>= 3.22.0'
dbus_glib_req = '>= 0.76'

glib_dep = dependency('glib-2.0', version: glib_req)
gio_dep = dependency('gio-2.0', version: gio_req)
gtk_dep = dependency('gtk+-3.0', version: gtk_req)
dbus_glib_dep = dependency('dbus-glib-1', version: dbus_glib_req)

x11_dep = dependency('x11')
sm_dep = dependency('sm')
ice_dep = dependency('ice')
xext_dep = dependency('xext')
xau_dep = dependency('xau')
xcomposite_dep = dependency('xcomposite')
gl_dep = dependency('gl')
epoxy_dep = dependency('epoxy')

# Optional components
gles_dep = dependency('', required: false)
egl_dep = dependency('', required: false)
if get_option('glesv2')
  gles_dep = dependency('glesv2', required: false)
  egl_dep = dependency('egl', required: false)
endif

libcanberra_dep = dependency('', required: false)
if get_option('libcanberra')
  libcanberra_dep = dependency('libcanberra-gtk3', required: false)
endif

systemd_dep = dependency('', required: false)
if get_option('systemd') and not get_option('elogind')
  systemd_dep = dependency('libsystemd', required: false)
  if not systemd_dep.found()
    systemd_dep = dependency('libsystemd-login', required: false)
  endif
endif

elogind_dep = dependency('', required: false)
if get_option('elogind')
  elogind_dep = dependency('libelogind', required: false)
endif

xtst_dep = dependency('xtst')
xrender_dep = dependency('xrender')

xtrans_dep = dependency('', required: false)
if get_option('xtrans')
  xtrans_dep = dependency('xtrans', required: false)
endif

# Configuration Data
conf = configuration_data()
conf.set_quoted('PACKAGE', meson.project_name())
conf.set_quoted('VERSION', meson.project_version())
conf.set_quoted('PACKAGE_NAME', meson.project_name())
conf.set_quoted('PACKAGE_VERSION', meson.project_version())
conf.set_quoted('PACKAGE_BUGREPORT', 'https://mate-desktop.org/')
conf.set_quoted('PACKAGE_STRING', '@0@ @1@'.format(meson.project_name(), meson.project_version()))
conf.set_quoted('GETTEXT_PACKAGE', meson.project_name())

# Headers
foreach h : ['syslog.h', 'tcpd.h', 'sys/param.h', 'execinfo.h']
  if cc.has_header(h)
    conf.set('HAVE_' + h.underscorify().to_upper(), 1)
  endif
endforeach

# Execinfo / backtrace
have_backtrace = false
execinfo_lib = []
if conf.has('HAVE_EXECINFO_H')
  if cc.has_function('backtrace')
    have_backtrace = true
  else
    execinfo_dep = cc.find_library('execinfo', required: false)
    if execinfo_dep.found() and cc.has_function('backtrace', dependencies: execinfo_dep)
      have_backtrace = true
      execinfo_lib = [execinfo_dep]
    endif
  endif
endif
if have_backtrace
  conf.set('HAVE_BACKTRACE', 1)
endif

# Feature flags
if get_option('ipv6')
  if cc.compiles('''
    #include <sys/types.h>
    #include <sys/socket.h>
    void f() { socket(AF_INET6, SOCK_STREAM, 0); }
  ''')
    conf.set('ENABLE_IPV6', 1)
  endif
endif

if gles_dep.found()
  conf.set('HAVE_GLESV2', 1)
endif

if libcanberra_dep.found()
  conf.set('HAVE_LIBCANBERRA', 1)
endif

if systemd_dep.found() or elogind_dep.found()
  conf.set('HAVE_SYSTEMD', 1)
endif

if elogind_dep.found()
  conf.set('HAVE_ELOGIND', 1)
endif

if xtst_dep.found()
  conf.set('HAVE_XTEST', 1)
endif

if xrender_dep.found()
  conf.set('HAVE_XRENDER', 1)
endif

if xtrans_dep.found()
  conf.set('HAVE_XTRANS', 1)
endif

# Check for XSync extension header
if cc.has_header('X11/extensions/sync.h', prefix: '#include <X11/Xlib.h>')
  conf.set('HAVE_XSYNC', 1)
endif

conf.set_quoted('DEFAULT_WM', get_option('default_wm'))
conf.set('STDC_HEADERS', 1)

# Generate config.h
configure_file(
  output: 'config.h',
  configuration: conf
)

# Global compiler flags
add_project_arguments('-DHAVE_CONFIG_H', language: 'c')

# Includes
include_dirs = [include_directories('.')]

# Subdirs
config_dep = declare_dependency(include_directories: include_directories('.'))
libegg_deps = [
  config_dep,
  glib_dep,
  gtk_dep,
  ice_dep,
  sm_dep,
]
subdir('mate-submodules/libegg')
libegg_dep = declare_dependency(
  include_directories: include_directories('mate-submodules', 'mate-submodules/libegg'),
  link_with: egg
)
subdir('mate-session')
subdir('capplet')
subdir('tools')
subdir('data')
subdir('po')
