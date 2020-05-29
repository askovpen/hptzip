project('hptzip','c', version:'1.9.0')

cc = meson.get_compiler('c')

add_project_arguments(
  '-I'+meson.current_source_dir()+'/hptzip',
  language : 'c')

link_args = []
compile_args = []

if cc.get_id() == 'msvc'
  add_project_arguments(
    '-D_CRT_SECURE_NO_DEPRECATE',
    '-D_CRT_NONSTDC_NO_DEPRECATE', 
    '-DWIN32',
    '-D_WINDOWS',
    '-D_CONSOLE',
    language : 'c')
  if get_option('default_library') != 'static'
    add_project_arguments('-D_DLL', '-D_MAKE_DLL', language : 'c')
  endif

else
  add_project_arguments(
    '-DUNIX',
  language : 'c')
endif

src=files([
  'src/hptzip.c',
  'src/ioapi.c',
  'src/unzip.c',
  'src/zip.c'
])

if cc.get_id() == 'msvc'
  src+=files(['src/iowin32.c'])
endif

headers = files([
  'hptzip/crypt.h',
  'hptzip/hptzipdl.h',
  'hptzip/hptzip.h',
  'hptzip/ioapi.h',
  'hptzip/iowin32.h',
  'hptzip/unzip.h',
  'hptzip/version.h',
  'hptzip/zip.h'

])

incdir = include_directories(['.'])

huskylib_proj = subproject('huskylib')
huskylib_dep=huskylib_proj.get_variable('husky_dep')

zlib=dependency('zlib')

if not zlib.found()
  zlib_proj = subproject('zlib') #.get_variable('zlib_dep')
  zlib=zlib_proj.get_variable('zlib_dep')
else
  if cc.has_header_symbol('zlib.h','_Z_OF')
    add_project_arguments(
      '-DOF=_Z_OF',
      language : 'c')
  endif
endif

libhptzip = library('hptzip', src,
  include_directories : incdir,
  dependencies:[huskylib_dep,zlib],
  version:'1.9.0',
  install : true)

hptzip_dep = declare_dependency(
  link_with: libhptzip,
  dependencies: [huskylib_dep,zlib],
  include_directories : incdir)

install_headers(headers, subdir: 'hptzip')
