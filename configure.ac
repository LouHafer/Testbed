# Copyright (C) 2020 Lou Hafer
# All Rights Reserved.
# This file is distributed under the Eclipse Public License.
#
# Author:  Lou Hafer	December 2020

#############################################################################
#                       Names and other basic things                        #
#############################################################################

AC_INIT([Testbed],[devel],[],[],[])

AC_COPYRIGHT([
Copyright 2020 Lou Hafer
All Rights Reserved.
This file is part of the open source package BuildTools Testbed which is
distributed under the Eclipse Public License.])

# List one file in the package so that the configure script can test whether
# the package is actually there.
AC_CONFIG_SRCDIR(src/mkltest.cpp)

# Do some initialization work (version numbers, change prefix default, ...)
AC_COIN_INITIALIZE

#############################################################################
#                         Standard build tool stuff                         #
#############################################################################

# Get the name of the C++ compiler and appropriate compiler options.
AC_COIN_PROG_CXX

# This is a C++ package, set the language accordingly.
AC_LANG_PUSH(C++)

# Initialize libtool.
AC_COIN_PROG_LIBTOOL

#############################################################################
#                   Math headers                                            #
#############################################################################

# Check for cmath/math.h, cfloat/float.h, cieeefp/ieeefp.h
AC_COIN_CHECK_MATH_HDRS

#############################################################################
#                   The Good Stuff                                          #
#############################################################################

MKLROOT=/devel/InstRoot/Intel/mkl

# Look in coin_chk_libhdr.m4 for the macro definitions and full documentation.
# The old COIN_CHK_LIB is renamed to COIN_CHK_LIB_OBS.

# CHK_LIB is just a wrapper around CHK_LIBHDR. By default, it will run a
# separate tests: a compile test on the header and a link test on the
# function, declared as void func(). The compile test uses the supplied
# compiler flags, the link test uses the supplied link flags.

# CHK_LIB in normal usage, will run a compile test using the code
#  #include "mkl.h"
# and a separate link test using the code
#  #ifdef __cplusplus
#    extern "C"
#  #endif
#    void dgemm() ;
#  
#  int main () { dgemm() ; return (0) ; }

AC_COIN_CHK_LIB(MKL,MklTest,
  [-L${MKLROOT}/lib/intel64 -lmkl_intel_ilp64 -lmkl_sequential -lmkl_core],
  [-DMKL_ILP64 -m64 -I${MKLROOT}/include],[],
  [dgemm],[mkl.h])

# Run the compile test only.

AC_COIN_CHK_LIB(MKL2,Mkl2Test,
  [],
  [-DMKL_ILP64 -m64 -I${MKLROOT}/include],[],
  [],[mkl.h])

# Run the link test only.

AC_COIN_CHK_LIB(MKL3,Mkl3Test,
  [-L${MKLROOT}/lib/intel64 -lmkl_intel_ilp64 -lmkl_sequential -lmkl_core],
  [],[],
  [dgemm],[])

# Trust the user, just set variables.

AC_COIN_CHK_LIB(MKL4,Mkl4Test,
  [-L${MKLROOT}/lib/intel64 -lmkl_intel_ilp64 -lmkl_sequential -lmkl_core],
  [-DMKL_ILP64 -m64 -I${MKLROOT}/include],[],
  [],[])

# Now, the underlying LIBHDR macro. By default, this runs a separate compile
# test on whatever is specified in [includes], using the underlying macro call
#   AC_LANG_PROG([includes],[])
# then a link test using
#   AC_LANG_PROG([includes],[function-body])
# You can request LIBHDR to run separate tests for compile and link. The
# compile test is unchanged, but the link test runs
#   AC_LANG_SOURCE([function-body])
# This asymmetry is required to support CHK_LIB as a wrapper. AC_LANG_PROG
# supplies the int main () {   } main function wrapper, and it's not legal to
# wrap the extern "C" void func() stuff inside the main function.

# This runs a compile check on mkl.h, then a link check using both [include]
# and [function-body], with the supplied compile and link flags. Works for
# things like C++ overloads.

AC_COIN_CHK_LIBHDR(MKL5,Mkl5Test,
  [-L${MKLROOT}/lib/intel64 -lmkl_intel_ilp64 -lmkl_sequential -lmkl_core],
  [-DMKL_ILP64 -m64 -I${MKLROOT}/include],[],
  [const char *transa, *transb ;
   const double *dummy1, *dummy2, *dummy3, *dummy4 ;
   double *dummy5 ;
   const MKL_INT *n, *m, *k, *l, *p, *q ;
   dgemm(transa,transb,n,m,k,dummy1,dummy2,l,dummy3,p,dummy4,dummy5,q) ;],
  [#include "mkl.h"])

# Header compile check only.

AC_COIN_CHK_LIBHDR(MKL6,Mkl6Test,
  [],
  [-DMKL_ILP64 -m64 -I${MKLROOT}/include],[],
  [],
  [#include "mkl.h"])

# This duplicates the void func() redeclaration but notice we've lost the
# header file. The void redeclaration is moved to the [includes] portion.

AC_COIN_CHK_LIBHDR(MKL7,Mkl7Test,
  [-L${MKLROOT}/lib/intel64 -lmkl_intel_ilp64 -lmkl_sequential -lmkl_core],
  [],[],
  [dgemm()],
  [#ifdef __cplusplus
extern "C"
#endif
void dgemm() ;])

# Trust the user and just set variables.

AC_COIN_CHK_LIBHDR(MKL8,Mkl8Test,
  [-L${MKLROOT}/lib/intel64 -lmkl_intel_ilp64 -lmkl_sequential -lmkl_core],
  [-DMKL_ILP64 -m64 -I${MKLROOT}/include],[],
  [],[])

# Here's how the CHK_LIB as wrapper idea really works. I overloaded the
# [dfltaction] parameter with a second part that specifies whether the link
# check should include the header or run separately. The [dfltaction] =
# yessep says `use this package by default, and do separate compile and
# link checks'.  So what happens here is that the header file compile
# check runs as AC_LANG_PROG([includes],[]) and the link check runs as
# AC_LANG_SOURCE([function-body]). Note that this particular invocation
# will return `no (header compile)' as a result, even though the link check
# succeeds, because I didn't supply the compile flags. This is the problem
# with Osi configure that got us here in the first place.

AC_COIN_CHK_LIBHDR(MKL9,Mkl9Test,
  [-L${MKLROOT}/lib/intel64 -lmkl_intel_ilp64 -lmkl_sequential -lmkl_core],
  [],[],
  [#ifdef __cplusplus
extern "C"
#endif
void dgemm() ;

int main () {
dgemm() ;
return (0) ; }],[#include "mkl.h"],yessep)

# This call will indicate failure for link and compile because the necessary
# lflags and cflags are not present.

AC_COIN_CHK_LIB(MKL10,Mkl10Test,
  [],[],[],
  [dgemm],[mkl.h],yessep)

# Just to check that default do not use works. Would fail if forced because
# of the notog spec, which just isn't appropriate from CHK_LIB.

AC_COIN_CHK_LIB(MKL11,Mkl11Test,
  [],[],[],
  [dgemm],[mkl.h],notog)

# Use this to test that command line flags override a default do not use.
# Specifying just --with-mkl12 will fail. Specifying correct cflags or lflags
# will pass compile or link, respectively.

AC_COIN_CHK_LIB(MKL12,Mkl12Test,
  [],[],[],
  [dgemm],[mkl.h],no)

##############################################################################
#                   Finishing up by writing all the output                   #
##############################################################################

# Here list all the files that configure should create (except for the
# configuration header file)

AC_CONFIG_FILES([Makefile
                 src/Makefile])

# Finally, we let configure write all the output...
AC_COIN_FINALIZE
