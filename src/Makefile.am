# Copyright (C) 2006 International Business Machines and others.
# All Rights Reserved.
# This file is distributed under the Eclipse Public License.
#
# Author:  Andreas Waechter           IBM    2006-04-13

include $(top_srcdir)/BuildTools/Makemain.inc

########################################################################
#                          Stuff to Build                              #
########################################################################

noinst_PROGRAMS = mkltest

mkltest_SOURCES = mkltest.cpp

AM_CPPFLAGS = $(MKLTEST_CFLAGS)
mkltest_LDADD = $(MKLTEST_LFLAGS)

AM_LDFLAGS = $(LT_LDFLAGS)

test: mkltest
	MKLROOT=/devel/InstRoot/Intel/mkl ; \
	LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:${MKLROOT}/lib/intel64 ; \
	export LD_LIBRARY_PATH ; \
	mkltest

.PHONY: test
