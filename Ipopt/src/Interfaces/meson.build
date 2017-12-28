headers = [
  'IpAlgTypes.hpp',
  'IpIpoptApplication.hpp',
  'IpNLP.hpp',
  'IpReturnCodes.h',
  'IpReturnCodes.hpp',
  'IpReturnCodes_inc.h',
  'IpReturnCodes.inc',
  'IpSolveStatistics.hpp',
  'IpStdCInterface.h',
  'IpTNLP.hpp',
  'IpTNLPAdapter.hpp',
  'IpTNLPReducer.hpp',
]
install_headers(headers, subdir: 'coin')

sources = [
  'IpInterfacesRegOp.cpp',
  'IpIpoptApplication.cpp',
  'IpSolveStatistics.cpp',
  'IpStdCInterface.cpp',
  'IpStdFInterface.c',
  'IpStdInterfaceTNLP.cpp',
  'IpTNLPAdapter.cpp',
  'IpTNLPReducer.cpp',
]

libipoptiface = static_library(
  'ipoptiface', sources,
  include_directories: include_directories('.', '../Common',
    '../LinAlg', '../LinAlg/TMatrices',
    '../Algorithm', '../Algorithm/LinearSolvers',
    '../Algorithm', '../Algorithm/Inexact',
    '../contrib/CGPenalty', '../contrib/LinearSolverLoader'),
  install: false)

# FIXME '$(HSL_CFLAGS)

ipopt_libs += [libipoptiface]
