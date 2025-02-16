
SRC = $(wildcard *.cpp)
OBJ = $(SRC:.cpp=.o)
EXE = gemm

ROCM_PATH ?= /opt/rocm
HIPCC      = ${ROCM_PATH}/bin/hipcc

HIP_INC  = -I${ROCM_PATH}/include
HIP_INC += -I${ROCM_PATH}/include/hiprand
HIP_INC += -I${ROCM_PATH}/include/rocrand

HIP_LIB  = -L${ROCM_PATH}/lib
HIP_LIB += -lhipblas -lhiprand
HIP_LIB += -lrocblas -lrocrand

CXXFLAGS       = -O3 -std=c++11
AMDGPU_TARGET  = $(shell ${ROCM_PATH}/bin/rocminfo | grep -m 1 -o -P 'gfx.{0,4}')
CXXFLAGS      += --amdgpu-target=${AMDGPU_TARGET}

CUDA_PATH ?= /usr/local/cuda
NVCC       = ${CUDA_PATH}/bin/nvcc

CUDA_LIB   = -lcublas -lcurand
CUDA_FLAGS = -x cu --expt-relaxed-constexpr

INC = $(HIP_INC)
LIB = $(HIP_LIB) $(ROC_LIBS)

rocm: $(OBJ)
	$(HIPCC) $(CXXFLAGS) $(INC) $(LIB) $(OBJ) -o $(EXE)

.cpp.o:
	$(HIPCC) $(CXXFLAGS) $(INC) -c $< -o $@

cuda:
	$(NVCC) $(CUDA_FLAGS) $(CUDA_LIB) $(SRC) -o $(EXE)

clean:
	rm -rf $(OBJ) $(EXE)
