sources = [
  'IpInexactAlgBuilder.cpp',
  'IpInexactCq.cpp',
  'IpInexactData.cpp',
  'IpInexactDoglegNormal.cpp',
  'IpInexactLSAcceptor.cpp',
  'IpInexactNewtonNormal.cpp',
  'IpInexactNormalTerminationTester.cpp',
  'IpInexactPDSolver.cpp',
  'IpInexactPDTerminationTester.cpp',
  'IpInexactRegOp.cpp',
  'IpInexactSearchDirCalc.cpp',
  'IpInexactTSymScalingMethod.cpp',
  'IpIterativePardisoSolverInterface.cpp',
  'IpIterativeSolverTerminationTester.cpp',
]

incdirs = ['.', '../../Common',
           '../../LinAlg', '../../LinAlg/TMatrices',
           '../../Interfaces',
           '..', '../LinearSolvers',
           '../../contrib/LinearSolverLoader']

# FIXME
#if COIN_HAS_HSL
#AM_CPPFLAGS += $(HSL_CFLAGS)
#endif

libinexact = static_library(
  'inexact', sources,
  include_directories: include_directories(incdirs),
  install: false)

ipopt_libs += [libinexact]
