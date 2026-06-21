icon_sizes = ['16x16', '22x22', '24x24', '32x32', '48x48']
foreach size : icon_sizes
  install_data(
    join_paths(size, 'apps', 'mate-session-properties.png'),
    install_dir: join_paths(datadir, 'icons', 'hicolor', size, 'apps')
  )
endforeach

install_data(
  join_paths('scalable', 'apps', 'mate-session-properties.svg'),
  install_dir: join_paths(datadir, 'icons', 'hicolor', 'scalable', 'apps')
)
