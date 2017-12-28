install_headers('IpSymLinearSolver.hpp', subdir : 'coin')

sources = [
  'IpLinearSolversRegOp.cpp',
  'IpSlackBasedTSymScalingMethod.cpp',
  'IpTripletToCSRConverter.cpp',
  'IpTSymDependencyDetector.cpp',
  'IpTSymLinearSolver.cpp',
]

sources += [
  'IpMa27TSolverInterface.cpp',
  'IpMa57TSolverInterface.cpp',
  'IpMa86SolverInterface.cpp',
  'IpMa97SolverInterface.cpp',
  'IpMc19TSymScalingMethod.cpp',
  'IpMa77SolverInterface.cpp',

  'IpMa28TDependencyDetector.cpp',
  'IpMa28Partition.F',
  'IpPardisoSolverInterface.cpp',
]

solver_deps = []

if Pardiso_dep.found()
  sources += []
  solver_deps += [Pardiso_dep]
endif
if MA28_dep.found()
  solver_deps += [MA28_dep]
endif
if WSMP_dep.found()
  sources += ['IpWsmpSolverInterface.cpp',
              'IpIterativeWsmpSolverInterface.cpp']
  solver_deps += [WSMP_dep]
endif
if MUMPS_dep.found()
  sources += ['IpMumpsSolverInterface.cpp']
  solver_deps += [MUMPS_dep]
endif
if HSL_dep.found()
  solver_deps += [HSL_dep]
endif

incdirs = ['.', '../../Common',
           '../../LinAlg', '../../LinAlg/TMatrices',
           '../../Interfaces',
           '..',
           '../../contrib/CGPenalty']

liblinsolvers = static_library(
  'linsolvers', sources,
  dependencies: solver_deps,
  include_directories: include_directories(incdirs),
  install: false)

ipopt_libs += [liblinsolvers]
ipopt_deps += solver_deps
