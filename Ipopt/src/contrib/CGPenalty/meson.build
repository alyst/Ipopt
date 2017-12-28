# no headers?

sources = [
  'IpCGPenaltyCq.cpp',
  'IpCGPenaltyData.cpp',
  'IpCGPenaltyLSAcceptor.cpp',
  'IpCGPenaltyRegOp.cpp',
  'IpCGPerturbationHandler.cpp',
  'IpCGSearchDirCalc.cpp',
  'IpPiecewisePenalty.cpp'
]

libcgpenalty = static_library(
  'cgpenalty', sources,
  include_directories: include_directories('.', '../../Common', '../../LinAlg',
                                            '../../LinAlg/TMatrices', '../../Interfaces',
                                            '../../Algorithm'),
  install: false)

ipopt_libs += [libcgpenalty]
