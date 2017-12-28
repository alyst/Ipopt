incdirs = include_directories(
    '../../Common', '../../LinAlg',
    '../../LinAlg/TMatrices', '../../Algorithm',
    '../../Interfaces')

libipoptamplinterface = static_library(
  'ipoptamplinterface', sources: ['AmplTNLP.cpp'],
  include_directories: incdirs,
  dependencies: [ASL_dep]
)

ipopt_libs += [libipoptamplinterface]

ipopt_bin = executable('ipopt',
  sources: 'ampl_ipopt.cpp',
  link_with: [libipoptamplinterface, libipopt],
  include_directories: incdirs,
  dependencies: ipopt_deps,
  install: true
)

install_headers('AmplTNLP.hpp', subdir: 'coin')

pkg.generate(
  filebase: 'ipoptamplinterface',
  name: 'IPOPT',
  description: 'Ampl Interface TNLP to Ipopt',
  url: 'https://projects.coin-or.org/Ipopt',
  version: '@PACKAGE_VERSION@', # FIXME
  libraries: 'lipoptamplinterface', # FIXME @IPOPTAMPLINTERFACELIB_PCLIBS@
  subdirs: ['coin'],
  requires: 'ipopt', # FIXME @IPOPTAMPLINTERFACELIB_PCREQUIRES@
)
