project('Ipopt', ['c', 'cpp'], version: '3.12.8')

pkg = import('pkgconfig')

add_project_arguments('-DIPOPT_BUILD=1', language: ['c', 'cpp'])
add_project_arguments('-DHAVE_CONFIG_H=1', language: ['c', 'cpp'])

cdata = configuration_data()

cdata.set('IPOPT_VERSION', '"'+meson.project_version()+'"')
ver_parts = meson.project_version().split('.')
cdata.set('IPOPT_VERSION_MAJOR', ver_parts[0])
cdata.set('IPOPT_VERSION_MINOR', ver_parts[1])
cdata.set('IPOPT_VERSION_RELEASE', ver_parts[2])

cc = meson.get_compiler('c')
cpp = meson.get_compiler('cpp')

if add_languages('fortran', required: false)
  has_fortran = true
  fortran = meson.get_compiler('fortran')
  # FIXME better mangling detection
  cdata.set('USE_FORTRAN_LIBS', 1)
  cdata.set('F77_FUNC_NAME', 'name ## _')
  cdata.set('F77_FUNC__NAME', 'name ## _')
else
  has_fortran = false
endif

# FIXME some meson function?
if target_machine.system() == 'cygwin'
  sharedlibext = 'dll'
elif target_machine.system() == 'darwin'
  sharedlibext = 'dylib'
else
  sharedlibext = 'so'
endif
cdata.set('SHAREDLIBEXT', '"'+sharedlibext+'"')

check_cpp_headers = [
  ['HAVE_ASSERT_H', 'assert.h'],
  ['HAVE_CASSERT', 'cassert'],
  ['HAVE_CCTYPE', 'cctype'],
  ['HAVE_CTYPE_H', 'ctype.h'],
  ['HAVE_CFLOAT', 'cfloat'],
  ['HAVE_FLOAT_H', 'float.h'],
  ['HAVE_CIEEEFP', 'cieeefp'],
  ['HAVE_IEEEFP_H', 'ieeefp.h'],
  ['HAVE_CMATH', 'cmath'],
  ['HAVE_MATH_H', 'math.h'],
  ['HAVE_CSTDARG', 'cstdarg'],
  ['HAVE_STDARG_H', 'stdarg.h'],
  ['HAVE_CSTDDEF', 'cstddef'],
  ['HAVE_STDDEF_H', 'stddef.h'],
  ['HAVE_STDINT_H', 'stdint.h'],
  ['HAVE_CSTDIO', 'cstdio'],
  ['HAVE_STDIO_H', 'stdio.h'],
  ['HAVE_CSTDLIB', 'cstdlib'],
  ['HAVE_STDLIB_H', 'stdlib.h'],
  ['HAVE_CSTRING', 'cstring'],
  ['HAVE_STRING_H', 'string.h'],
  ['HAVE_STRINGS_H', 'strings.h'],
  ['HAVE_CTIME', 'ctime'],
  ['HAVE_DLFCN_H', 'dlfcn.h'],
  ['HAVE_INTTYPES_H', 'inttypes.h'],
  ['HAVE_MEMORY_H', 'memory.h'],
  ['HAVE_SYS_STAT_H', 'sys/stat.h'],
  ['HAVE_SYS_TYPES_H', 'sys/types.h'],
  ['HAVE_TIME_H', 'time.h'],
  ['HAVE_UNISTD_H', 'unistd.h'],
  ['HAVE_WINDOWS_H', 'windows.h'],
]

foreach h : check_cpp_headers
  if cpp.has_header(h.get(1))
    cdata.set(h.get(0), 1)
  endif
endforeach

check_functions = [
  ['HAVE_DRAND48', 'drand48', '#include<stdlib.h>'],
  ['HAVE_RAND', 'rand', '#include<stdlib.h>'],
  ['HAVE_STD__RAND', 'rand', '#include<cstdlib>\nusing namespace std;'],
  ['HAVE_SNPRINTF', 'snprintf', '#include<stdio.h>'],
  ['HAVE__SNPRINTF', '_snprintf', '#include<stdio.h>'],
  ['HAVE_VSNPRINTF', 'vsnprintf', '#include<stdio.h>'],
  ['HAVE__VSNPRINTF', '_vsnprintf', '#include<stdio.h>'],
  ['HAVE_VA_COPY', 'va_copy', '#include<stdarg.h>'],
]

foreach f : check_functions
  if cpp.has_function(f.get(1), prefix: f.get(2))
    cdata.set(f.get(0), 1)
  endif
endforeach
if cpp.has_function('finite', prefix: '#include<cmath>')
  cdata.set('COIN_C_FINITE', 'finite')
endif

cdata.set('SIZEOF_INT_P', cc.sizeof('int*'))
cdata.set('FORTRAN_INTEGER_TYPE', cc.sizeof('int') <= cc.sizeof('double') ? 'int' : 'long')

if get_option('enable-inexact-solver')
  cdata.set('BUILD_INEXACT', 1)
  # FIXME IPALLLIBS="../Algorithm/Inexact/libinexact.la $IPALLLIBS"
endif

with_WSMP = get_option('with-wsmp')
if with_WSMP != ''
  if has_fortran
    WSMP_dep = cc.find_library(with_WSMP, required: false)
    if not WSMP_dep.found() or not fortran.links('call WSSMP();', dependencies: WSMP_dep, required: false)
      WSMP_dep = disabler()
      warning('WSMP not found')
    endif
  else
    warning('WSMP not used: no Fortran compiler')
    WSMP_dep = disabler()
  endif
else
  WSMP_dep = disabler()
endif

#mpi_dep = dependency('mpi', languag#e: 'cpp', required: false)
#if mpi_dep.found()
#  cdata.set('HAVE_MPI_INITIALIZED', 1)
#endif

libdl = cc.find_library('dl')

MKL_dep = disabler()

BLAS_dep = disabler()
custom_blas = false
if get_option('with-blas') != ''
  blas_args = get_option('with-blas').split(' ')
  BLAS_dep = declare_dependency(compile_args: blas_args, link_args: blas_args)
  if cc.has_function('daxpy', dependencies: BLAS_dep)
    custom_blas = true
    # check whether BLAS is from MKL
    if cc.has_function('mkl_get_version_string', dependencies: BLAS_dep)
      MKL_dep = BLAS_dep
    endif
  else
    warning('with-blas option doesn\'t provide BLAS')
    BLAS_dep = disabler()
  endif
endif

if not BLAS_dep.found()
  coinblas_sp = subproject('coinblas')
  BLAS_dep = coinblas_sp.get_variable('coinblas_dep')
endif

if BLAS_dep.found()
  cdata.set('COIN_HAS_BLAS', 1)
endif

LAPACK_dep = disabler()
custom_lapack = false
if get_option('with-lapack') != ''
  lapack_args = get_option('with-lapack').split(' ')
  LAPACK_dep = declare_dependency(compile_args: lapack_args, link_args: lapck_args)
  if cc.has_function('dsyev', dependencies: LAPACK_dep)
    custom_lapack = true
  else
    warning('with-lapack option doesn\'t provide LAPCK')
    LAPACK_dep = disabler()
  endif
endif

if not LAPACK_dep.found() and custom_blas
  # check whether BLAS_dep provides LAPACK
  if cc.has_function('dsyev', dependencies: BLAS_dep)
    LAPACK_dep = BLAS_dep
    custom_lapack = true
  endif
endif

if not LAPACK_dep.found()
  coinlapack_sp = subproject('coinlapack')
  LAPACK_dep = coinlapack_sp.get_variable('coinlapack_dep')
endif

if LAPACK_dep.found()
  cdata.set('COIN_HAS_LAPACK', 1)
endif

#dmumps_dep = cc.find_library('dmumps', required: false) # FIXME (with-mumps)
#if cc.has_header('mumps_mpi.h')
#  cdata.set('COIN_USE_MUMPS_MPI_H', 1)
#endif
#if not cc.has_header('dmumps_c.h')
#  dmumps_dep = disabler()
#endif
#if mpi_dep.found() and dmumps_dep.found()
#  MUMPS_dep = declare_dependency(dependencies: [mpi_dep, dmumps_dep])
#else
#  MUMPS_dep = disabler()
#endif

coinasl_sp = subproject('coinasl')
ASL_dep = coinasl_sp.get_variable('coinasl_dep')
if ASL_dep.found()
  cdata.set('COIN_HAS_ASL', 1)
endif

MA28_dep = disabler() # FIXME
HSL_dep = disabler() # FIXME
Pardiso_dep = disabler() # FIXME

pardiso_mkl = false
if not Pardiso_dep.found() and MKL_dep.found()
  # check whether MKL provides Pardiso
  if cc.has_function('PARDISO', dependencies: MKL_dep)
    message('Using Pardiso provided by MKL')
    Pardiso_dep = MKL_dep
    pardiso_mkl = true
    cdata.set('HAVE_PARDISO_MKL', 1)
  endif
endif
# check token ('HAVE_PARDISO_OLDINTERFACE',)
# check token ('HAVE_PARDISO_PARALLEL',)

if Pardiso_dep.found()
  cdata.set('HAVE_PARDISO', 1)
endif

build_LinearSolverLoader = has_fortran and get_option('enable-linear-solver-loader')
if build_LinearSolverLoader
  cdata.set('HAVE_LINEARSOLVERLOADER', 1)
endif

MUMPS_dep = disabler()
if not (Pardiso_dep.found() or MA28_dep.found() or HSL_dep.found() or build_LinearSolverLoader) or get_option('with-mumps') == 'bundled'
  coinmumps_sp = subproject('coinmumps')
  MUMPS_dep = coinmumps_sp.get_variable('coinmumps_dep')
  if MUMPS_dep.found()
    cdata.set('COIN_HAS_MUMPS', 1)
    #  cdata.set('COIN_USE_MUMPS_MPI_H', 1)
  endif
endif

if not (MUMPS_dep.found() or Pardiso_dep.found() or MA28_dep.found() or HSL_dep.found() or build_LinearSolverLoader)
  warning('No linear solvers will be built')
endif

cdata.set('COIN_IPOPT_CHECKLEVEL', get_option('with-ipopt-checklevel'))
cdata.set('COIN_IPOPT_VERBOSITY', get_option('with-ipopt-verbosity'))

# vars to accumulate local static libraries and their dependencies of subdirs
ipopt_libs = []
ipopt_deps = [libdl, BLAS_dep, LAPACK_dep, ASL_dep]

subdir('src/Common')

if build_LinearSolverLoader
  message('Building LinearSolverLoader: yes')
  subdir('src/contrib/LinearSolverLoader')
else
  message('Building LinearSolverLoader: no')
  LinearSolverLoader_dep = disabler()
endif

subdir('src/LinAlg')
subdir('src/Algorithm')
subdir('src/Interfaces')
subdir('src/contrib/CGPenalty')

ipopt_objs = []
foreach l : ipopt_libs
  ipopt_objs += l.extract_all_objects()
endforeach
libipopt = static_library('ipopt', objects: ipopt_objs, dependencies: ipopt_deps,
                          install: true)

if ASL_dep.found()
  subdir('src/Apps/AmplSolver')
endif
if false # FIXME
if has_fortran
  subdir('src/Apps/CUTErInterface')
endif
endif
