subdir('TMatrices')

headers = [
  'IpMatrix.hpp',
  'IpSymMatrix.hpp',
  'IpExpansionMatrix.hpp',
  'IpVector.hpp',
  'IpDenseVector.hpp',
  'IpCompoundVector.hpp',
  'IpCompoundMatrix.hpp',
  'IpCompoundSymMatrix.hpp',
  'IpSumSymMatrix.hpp',
  'IpDiagMatrix.hpp',
  'IpIdentityMatrix.hpp',
  'IpScaledMatrix.hpp',
  'IpSymScaledMatrix.hpp',
  'IpZeroSymMatrix.hpp',
  'IpBlas.hpp',
  'IpLapack.hpp',
]
install_headers(headers, subdir: 'coin')

sources = [
  'IpBlas.cpp',
  'IpCompoundMatrix.cpp',
  'IpCompoundSymMatrix.cpp',
  'IpCompoundVector.cpp',
  'IpDenseGenMatrix.cpp',
  'IpDenseSymMatrix.cpp',
  'IpDenseVector.cpp',
  'IpDiagMatrix.cpp',
  'IpExpandedMultiVectorMatrix.cpp',
  'IpExpansionMatrix.cpp',
  'IpIdentityMatrix.cpp',
  'IpLapack.cpp',
  'IpLowRankUpdateSymMatrix.cpp',
  'IpMatrix.cpp',
  'IpMultiVectorMatrix.cpp',
  'IpScaledMatrix.cpp',
  'IpSumMatrix.cpp',
  'IpSumSymMatrix.cpp',
  'IpSymScaledMatrix.cpp',
  'IpTransposeMatrix.cpp',
  'IpVector.cpp',
  'IpZeroMatrix.cpp',
  'IpZeroSymMatrix.cpp'
]
liblinalg = static_library(
  'linalg', sources,
  dependencies: [BLAS_dep],
  include_directories: include_directories('.', '../Common'),
  install: false)

ipopt_libs += [liblinalg]
