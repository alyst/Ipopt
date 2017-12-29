headers = [
  'HSLLoader.h',
]

sources = [
  'LibraryHandler.c',
  'HSLLoader.c',
]

loader_deps = []

if not pardiso_mkl
  headers += ['PardisoLoader.h']
  sources += ['PardisoLoader.c']
  loader_deps += [Pardiso_dep]
endif

install_headers(headers, subdir: 'coin')

libLinearSolverLoader = static_library(
  'LinearSolverLoader', sources,
  dependencies: loader_deps,
  include_directories: include_directories('.', '../../Common'),
  install: false)

LinearSolverLoader_dep = declare_dependency(link_with: libLinearSolverLoader)

ipopt_libs += [libLinearSolverLoader]
ipopt_deps += loader_deps
