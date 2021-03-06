SHELL=/bin/sh
BENCHMARK=mg
BENCHMARKU=MG
VERSION=00
OMP=no
ACC=no
NTHREADS=128
PROFILE_GENERATE=no
PROGRAM_LIBRARY=no
BOUNDS_CHECK=no
CUDA=no
ACC_MODEL=default

include ../config/make.def

OBJS = mg_v$(VERSION).o ${COMMON}/print_results.o  \
	${COMMON}/${RAND}.o ${COMMON}/timers.o ${COMMON}/wtime.o

ifeq ($(CUDA),yes)
# Add in the CUDA (N.B. no extra link flags needed)
 OBJS += mg_kernels.o 
 FFLAGS += -DUSE_CUDA
endif

include ../sys/make.common

${PROGRAM}: config ${OBJS}
	${FLINK} ${FLINKFLAGS} -o ${PROGRAM} ${OBJS} ${F_LIB}

mg_v$(VERSION).o: mg_v$(VERSION).f globals.h npbparams.h
	${FCOMPILE} -eF -DNTHREADS=$(NTHREADS) -o $@ $<

mg_kernels.o: mg_kernels.cu
	nvcc -DNTHREADS=$(NTHREADS) -arch sm_35 -O3 -c -o mg_kernels.o mg_kernels.cu

clean:
	- rm -f *.o *~ *.cub *.ptx *.lst *.cg *.opt
	- rm -f npbparams.h core
	- if [ -d rii_files ]; then rm -r rii_files; fi
