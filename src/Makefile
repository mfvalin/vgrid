#
# this Makefile has been set for "export" mode (outside of EC Dorval)
#
# the only "external" dependency is the shared library for rmnlib (only needed for make shared)
# make static 
# is self sufficient (no external dependencies)
#
ifeq ($(BASE_ARCH),$(EC_ARCH))
   $(error ERROR: please setup a compilation environment,  BASE_ARCH is the same as EC_ARCH )
endif

.SUFFIXES:

.SUFFIXES : .o .F90 .c .a

.c.o:
	s.cc -c ${CFLAGS} ${SUPP_OPT} -O ${OPTIMIZ} $<

.F90.o:
	s.f90 -c ${FFLAGS} ${SUPP_OPT} -O ${OPTIMIZ} $<

SHELL = /bin/bash

CFLAGS =

FFLAGS =

SUPP_OPT = -openmp

OPTIMIZ = 2

LIBSTATIC = libvgrid.a

LIBSHARED = libvgrid_shared.so

LIBRMNSHARED = rmn_shared

# install directory
INSTALL = ../..

.PRECIOUS:

OBJECTS = vgrid_utils.o vgrid.o vgrid_descriptors.o

# by default, make both static and shared version
default: ${LIBSTATIC} ${LIBSHARED}

static: ${LIBSTATIC}

shared: ${LIBSHARED}

obj:	$(OBJECTS)

install : ${LIBSTATIC} ${LIBSHARED}
	mkdir -p ${INSTALL}/lib/${EC_ARCH}
	mkdir -p ${INSTALL}/include/${EC_ARCH}
	cp ${LIBSTATIC} ${LIBSHARED} ${INSTALL}/lib/${EC_ARCH}
	cp *.mod vgrid.h ${INSTALL}/include/${EC_ARCH}

vgrid.o:	vgrid.c \
	BODY_C_compute_heights_4001.hc  BODY_C_compute_pressure_2001.hc  armnlib.h  BODY_C_compute_pressure_5100.hc  \
	BODY_Cvgd_diag_withref_2ref.hc BODY_C_compute_pressure_5002_5003_5004_5005.hc  vgrid.h  \
	BODY_C_compute_heights_0001.hc  BODY_C_compute_pressure_1003_5001.hc  BODY_C_compute_pressure_1001_1002.hc   \
	BODY_C_compute_heights_21001.hc  

vgrid_descriptors.o:	vgrid_descriptors.F90 \
	vgrid_utils.o  BODY_F_dpidpis_withref.hf  BODY_F_dpidpis_withref_prof.hf  BODY_F_diag_withref.hf  \
	BODY_F_levels_withref_prof.hf   BODY_F_levels_withref.hf  BODY_F_diag_withref_prof.hf  vgrid_descriptors.hf  

vgrid_utils.o:	vgrid_utils.F90 vgrid_descriptors.hf  


${LIBSHARED}: $(OBJECTS)
	rm -f $@
	s.f90 -shared $(OBJECTS) -l${LIBRMNSHARED} -o $@

${LIBSTATIC}: $(OBJECTS)
	rm -f $@
	ar rcv $@ $(OBJECTS)

clean:
	rm -f *.o *.mod *.a *.so
