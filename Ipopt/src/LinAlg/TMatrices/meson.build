headers = [
  'IpGenTMatrix.hpp',
  'IpSymTMatrix.hpp',
  'IpTripletHelper.hpp',
]
install_headers(headers, subdir: 'coin')

sources = [
  'IpGenTMatrix.cpp',
  'IpSymTMatrix.cpp',
  'IpTripletHelper.cpp',
]

libtmatrices = static_library(
  'tmatrices', sources,
  include_directories: include_directories('.', '..', '../../Common'),
  install: false)

ipopt_libs += [libtmatrices]
