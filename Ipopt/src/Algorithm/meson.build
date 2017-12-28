headers = [
  'IpIpoptCalculatedQuantities.hpp',
  'IpIpoptData.hpp', 'IpIteratesVector.hpp', 'IpTimingStatistics.hpp',
  'IpIpoptNLP.hpp', 'IpOrigIpoptNLP.hpp', 'IpNLPScaling.hpp',
  'IpAlgBuilder.hpp', 'IpIpoptAlg.hpp', 'IpAlgStrategy.hpp',
  'IpSearchDirCalculator.hpp', 'IpLineSearch.hpp', 'IpMuUpdate.hpp',
  'IpConvCheck.hpp', 'IpIterateInitializer.hpp', 'IpIterationOutput.hpp',
  'IpHessianUpdater.hpp', 'IpEqMultCalculator.hpp', 'IpAugSystemSolver.hpp',
  'IpPDSystemSolver.hpp'
]
install_headers(headers, subdir: 'coin')

sources = [
  'IpAdaptiveMuUpdate.cpp',
  'IpAlgBuilder.cpp',
  'IpAlgorithmRegOp.cpp',
  'IpAugRestoSystemSolver.cpp',
  'IpBacktrackingLineSearch.cpp',
  'IpDefaultIterateInitializer.cpp',
  'IpEquilibrationScaling.cpp',
  'IpExactHessianUpdater.cpp',
  'IpFilter.cpp',
  'IpFilterLSAcceptor.cpp',
  'IpGenAugSystemSolver.cpp',
  'IpGradientScaling.cpp',
  'IpIpoptAlg.cpp',
  'IpIpoptCalculatedQuantities.cpp',
  'IpIpoptData.cpp',
  'IpIteratesVector.cpp',
  'IpLeastSquareMults.cpp',
  'IpLimMemQuasiNewtonUpdater.cpp',
  'IpLoqoMuOracle.cpp',
  'IpLowRankAugSystemSolver.cpp',
  'IpLowRankSSAugSystemSolver.cpp',
  'IpMonotoneMuUpdate.cpp',
  'IpNLPBoundsRemover.cpp',
  'IpNLPScaling.cpp',
  'IpOptErrorConvCheck.cpp',
  'IpOrigIpoptNLP.cpp',
  'IpOrigIterationOutput.cpp',
  'IpPDFullSpaceSolver.cpp',
  'IpPDPerturbationHandler.cpp',
  'IpPDSearchDirCalc.cpp',
  'IpPenaltyLSAcceptor.cpp',
  'IpProbingMuOracle.cpp',
  'IpQualityFunctionMuOracle.cpp',
  'IpRestoConvCheck.cpp',
  'IpRestoFilterConvCheck.cpp',
  'IpRestoIpoptNLP.cpp',
  'IpRestoIterateInitializer.cpp',
  'IpRestoIterationOutput.cpp',
  'IpRestoMinC_1Nrm.cpp',
  'IpRestoPenaltyConvCheck.cpp',
  'IpRestoRestoPhase.cpp',
  'IpStdAugSystemSolver.cpp',
  'IpTimingStatistics.cpp',
  'IpUserScaling.cpp',
  'IpWarmStartIterateInitializer.cpp'
]

incdirs = ['.', './LinearSolvers', '../Common',
           '../LinAlg', '../LinAlg/TMatrices',
           '../Interfaces',
           '../contrib/CGPenalty',
           '../contrib/LinearSolverLoader']

libipoptalg = static_library(
  'ipoptalg', sources,
  include_directories: include_directories(incdirs),
  install: false)

subdir('LinearSolvers')
subdir('Inexact')

ipopt_libs += [libipoptalg]
