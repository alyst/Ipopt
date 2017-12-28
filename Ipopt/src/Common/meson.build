configure_file(input: 'config.h.meson',
  output: 'config.h',
  configuration: cdata)

configure_file(input: 'config_ipopt.h.meson',
  output: 'config_ipopt.h',
  configuration: cdata)

headers = [
  'IpCachedResults.hpp',
  'IpDebug.hpp',
  'IpException.hpp',
  'IpJournalist.hpp',
  'IpObserver.hpp',
  'IpOptionsList.hpp',
  'IpReferenced.hpp',
  'IpRegOptions.hpp',
  'IpSmartPtr.hpp',
  'IpTaggedObject.hpp',
  'IpTimedTask.hpp',
  'IpTypes.hpp',
  'IpUtils.hpp'
]
install_headers(headers, subdir: 'coin')

sources = [
  'IpDebug.cpp',
  'IpJournalist.cpp',
  'IpObserver.cpp',
  'IpOptionsList.cpp',
  'IpRegOptions.cpp',
  'IpTaggedObject.cpp',
  'IpUtils.cpp',
]

libcommon = static_library(
  'common', sources,
  include_directories: include_directories('.'),
  install: false)

ipopt_libs += [libcommon]
