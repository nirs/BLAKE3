project(
  'libblake3',
  'c',
  version: '1.4.1',
  default_options: [
    'buildtype=release',
  ]
)

subdir('c')

blake3_dep = declare_dependency(
  link_with : blake3_lib,
  include_directories : blake3_inc
)
