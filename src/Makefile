objects=cuLDPC.o cuLDPC_CPU.o

all: $(objects)
	nvcc -arch=sm_35 -D_FORCE_INLINES $(objects) -o app

%.o: %.cu
	nvcc -O2 -x cu -arch=sm_35 -I. -D_FORCE_INLINES -dc $< -o $@

%.o: %.cpp
	nvcc -O2 -x cu -arch=sm_35 -I/usr/local/cuda/include -D_FORCE_INLINES -dc $< -o $@

clean:
	rm -f *.o app

