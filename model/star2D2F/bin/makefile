#include header#
include $(CUMC3D_DIR)/make.inc

#specify HDF5#
EXTRAINCS += $(HDF5INCS)
EXTRAOBJECTS += $(HDF5LIBS)

#specify directories#
SOLVER := $(CUMC3D_DIR)/solver/mhd
MODEL  := ../src
CUSTOM := ../custom-mhd

vpath %.f90 $(CUSTOM):$(SOLVER):$(MODEL)

#source list#
SOURCE = $(shell cat $(SOLVER)/source.list) $(shell cat $(MODEL)/source.list)
SRC = $(notdir $(SOURCE))
OBJ = $(patsubst %.f90,%.o,$(SRC))

#core object files#
DEF_OBJ    = definition.o
RECON_MOD  = ppmc_module.o tvd_module.o weno_module.o
SIMPLE_MOD = rsolver_module.o mhd_module.o def.o				# Only depends on definition.o

OTHER_OBJ = $(filter-out $(RECON_MOD) $(SIMPLE_MOD) $(DEF_OBJ), $(OBJ))

#you can use make -j8 to build parallelly#
ALL: $(OBJ)
	$(F90) $(LDFLAGS) -o ../CUMC3D $(OBJ) $(EXTRAOBJECTS)

$(SIMPLE_MOD): $(DEF_OBJ)
$(RECON_MOD) : $(DEF_OBJ) $(SIMPLE_MOD)
$(OTHER_OBJ) : $(DEF_OBJ) $(SIMPLE_MOD) $(RECON_MOD)

%.o: %.f90
	$(F90) $(F90FLAGS) $(EXTRAINCS) -c $< -o $@
