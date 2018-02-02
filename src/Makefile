objects=cuLDPC.o cuLDPC_CPU.o
ALL_CCFLAGS += -Xcompiler -fopenmp

all: $(objects)
	nvcc -arch=sm_35 -D_FORCE_INLINES $(ALL_CCFLAGS) $(objects) -o app

%.o: %.cu
	nvcc -O2 -x cu -arch=sm_35 -I. -D_FORCE_INLINES $(ALL_CCFLAGS) -dc $< -o $@

%.o: %.cpp
	nvcc -O2 -x cu -arch=sm_35 -I/usr/local/cuda/include -D_FORCE_INLINES $(ALL_CCFLAGS) -dc $< -o $@

clean:
	rm -f *.o app

