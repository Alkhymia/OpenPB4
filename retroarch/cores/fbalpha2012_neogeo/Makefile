DEBUG = 0
LIBRETRO_OPTIMIZATIONS = 1
FRONTEND_SUPPORTS_RGB565 = 1
HAVE_GRIFFIN = 0

ifeq ($(platform),)
   platform = unix
   ifeq ($(shell uname -a),)
      platform = win
      EXE_EXT=.exe
   else ifneq ($(findstring Darwin,$(shell uname -a)),)
      platform = osx
   else ifneq ($(findstring MINGW,$(shell uname -a)),)
      platform = win
   endif
else ifneq (,$(findstring armv,$(platform)))
   override platform += unix
else ifneq (,$(findstring rpi,$(platform)))
   override platform += unix
endif

MAIN_FBA_DIR := src
FBA_BURN_DIR := $(MAIN_FBA_DIR)/burn
FBA_BURN_DRIVERS_DIR := $(MAIN_FBA_DIR)/burn/drv
FBA_BURNER_DIR := $(MAIN_FBA_DIR)/burner
LIBRETRO_DIR := $(FBA_BURNER_DIR)/libretro
FBA_CPU_DIR := $(MAIN_FBA_DIR)/cpu
FBA_LIB_DIR := $(MAIN_FBA_DIR)/dep/libs
FBA_INTERFACE_DIR := $(MAIN_FBA_DIR)/intf
FBA_GENERATED_DIR = $(MAIN_FBA_DIR)/dep/generated
FBA_SCRIPTS_DIR = $(MAIN_FBA_DIR)/dep/scripts
GRIFFIN_DIR = griffin-libretro

EXTERNAL_ZLIB = 0

TARGET_NAME := fbalpha2012_neogeo
BURN_BLACKLIST :=
FBA_LIBRETRO_DIRS := $(LIBRETRO_DIR)

TARGET := $(TARGET_NAME)_libretro.so
fpic := -fPIC
SHARED := -shared -Wl,-no-undefined -Wl,--version-script=$(LIBRETRO_DIR)/link.T
PLATFORM_DEFINES += -fomit-frame-pointer -ffast-math
PLATFORM_DEFINES += -DARM
CXXFLAGS += -fno-rtti -fno-exceptions
PLATFORM_DEFINES += -marm -mcpu=cortex-a9 -mfpu=neon -mfloat-abi=hard

CC_SYSTEM = gcc
CXX_SYSTEM = g++

.PHONY: clean generate-files generate-files-clean clean-objs

all: $(TARGET)

BURN_BLACKLIST += $(FBA_BURNER_DIR)/un7z.cpp \
	$(FBA_CPU_DIR)/arm7/arm7exec.c \
	$(FBA_CPU_DIR)/arm7/arm7core.c \
	$(FBA_CPU_DIR)/hd6309/6309tbl.c \
	$(FBA_CPU_DIR)/hd6309/6309ops.c \
	$(FBA_CPU_DIR)/konami/konamtbl.c \
	$(FBA_CPU_DIR)/konami/konamops.c \
	$(FBA_CPU_DIR)/m68k/m68k_in.c \
	$(FBA_CPU_DIR)/m6800/6800ops.c \
	$(FBA_CPU_DIR)/m6800/6800tbl.c \
	$(FBA_CPU_DIR)/m6805/6805ops.c \
	$(FBA_CPU_DIR)/m6809/6809ops.c \
	$(FBA_CPU_DIR)/m6809/6809tbl.c \
	$(FBA_CPU_DIR)/sh2/mksh2.cpp \
	$(FBA_CPU_DIR)/sh2/mksh2-x86.cpp \
	$(FBA_CPU_DIR)/m68k/m68kmake.c \
	$(FBA_BURNER_DIR)/wave_writer.cpp \
	$(FBA_CPU_DIR)/m68k/m68kdasm.c \
	$(FBA_LIBRETRO_DIR)/menu.cpp \
	$(FBA_CPU_DIR)/sh2/mksh2.cpp \
	$(FBA_BURNER_DIR)/sshot.cpp \
	$(FBA_BURNER_DIR)/conc.cpp \
	$(FBA_BURNER_DIR)/dat.cpp \
	$(FBA_BURNER_DIR)/cong.cpp \
	$(FBA_BURNER_DIR)/image.cpp \
	$(FBA_BURNER_DIR)/misc.cpp \
	$(FBA_CPU_DIR)/h6280/tblh6280.c \
	$(FBA_CPU_DIR)/m6502/t65sc02.c \
	$(FBA_CPU_DIR)/m6502/t65c02.c \
	$(FBA_CPU_DIR)/m6502/tdeco16.c \
	$(FBA_CPU_DIR)/m6502/tn2a03.c \
	$(FBA_CPU_DIR)/m6502/t6502.c \
	$(FBA_CPU_DIR)/nec/v25sfr.c \
	$(FBA_CPU_DIR)/nec/v25instr.c \
	$(FBA_CPU_DIR)/nec/necinstr.c \
	$(FBA_BURN_DIR)/drv/capcom/ctv_make.cpp \
	$(FBA_BURN_DIR)/drv/pgm/pgm_sprite_create.cpp

#ifeq ($(LIBRETRO_OPTIMIZATIONS), 1)
#BURN_BLACKLIST += $(FBA_BURN_DIR)/drv/capcom/ctv.cpp
#endif

ifeq ($(HAVE_GRIFFIN), 1)
GRIFFIN_CXXSRCFILES := $(GRIFFIN_DIR)/cps12.cpp $(GRIFFIN_DIR)/cps3.cpp $(GRIFFIN_DIR)/neogeo.cpp $(GRIFFIN_DIR)/pgm.cpp $(GRIFFIN_DIR)/snes.cpp $(GRIFFIN_DIR)/galaxian.cpp $(GRIFFIN_DIR)/cpu-m68k.cpp
BURN_BLACKLIST += $(FBA_CPU_DIR)/m68000_intf.cpp
else
CPS2_DIR := $(FBA_BURN_DRIVERS_DIR)/capcom
CPS3_DIR := $(FBA_BURN_DRIVERS_DIR)/cps3
GALAXIAN_DIR := $(FBA_BURN_DRIVERS_DIR)/galaxian
NEOGEO_DIR := $(FBA_BURN_DRIVERS_DIR)/neogeo
PGM_DIR := $(FBA_BURN_DRIVERS_DIR)/pgm
SNES_DIR := $(FBA_BURN_DRIVERS_DIR)/snes
M68K_DIR := $(FBA_CPU_DIR)/m68k
MD_DIR := $(FBA_BURN_DRIVERS_DIR)/megadrive
PCE_DIR := $(FBA_BURN_DRIVERS_DIR)/pce
endif

ifeq ($(NO_MD), 1)
MD_DIR :=
endif

ifeq ($(NO_PCE), 1)
PCE_DIR :=
endif

ifeq ($(NO_CPS), 1)
BURN_BLACKLIST += $(FBA_BURN_DRIVERS_DIR)/capcom/cps.cpp \
						$(FBA_BURN_DRIVERS_DIR)/capcom/cps2_crpt.cpp \
						$(FBA_BURN_DRIVERS_DIR)/capcom/cps_config.cpp \
						$(FBA_BURN_DRIVERS_DIR)/capcom/cps_draw.cpp \
						$(FBA_BURN_DRIVERS_DIR)/capcom/cps_mem.cpp \
						$(FBA_BURN_DRIVERS_DIR)/capcom/cps_obj.cpp \
						$(FBA_BURN_DRIVERS_DIR)/capcom/cps_pal.cpp \
						$(FBA_BURN_DRIVERS_DIR)/capcom/cps_run.cpp \
						$(FBA_BURN_DRIVERS_DIR)/capcom/cps_rw.cpp \
						$(FBA_BURN_DRIVERS_DIR)/capcom/cps_scr.cpp \
						$(FBA_BURN_DRIVERS_DIR)/capcom/cpsr.cpp \
						$(FBA_BURN_DRIVERS_DIR)/capcom/cpsrd.cpp \
						$(FBA_BURN_DRIVERS_DIR)/capcom/cpst.cpp \
						$(FBA_BURN_DRIVERS_DIR)/capcom/ctv.cpp \
						$(FBA_BURN_DRIVERS_DIR)/capcom/d_cps1.cpp \
						$(FBA_BURN_DRIVERS_DIR)/capcom/d_cps2.cpp \
						$(FBA_BURN_DRIVERS_DIR)/capcom/fcrash_snd.cpp \
						$(FBA_BURN_DRIVERS_DIR)/capcom/ps.cpp \
						$(FBA_BURN_DRIVERS_DIR)/capcom/ps_m.cpp \
						$(FBA_BURN_DRIVERS_DIR)/capcom/ps_z.cpp \
						$(FBA_BURN_DRIVERS_DIR)/capcom/qs.cpp \
						$(FBA_BURN_DRIVERS_DIR)/capcom/qs_c.cpp \
						$(FBA_BURN_DRIVERS_DIR)/capcom/qs_z.cpp \
						$(FBA_BURN_DRIVERS_DIR)/capcom/sf2mdt_snd.cpp
endif

ifeq ($(NO_NEO), 1)
NEOGEO_DIR :=
endif

FBA_BURN_DIRS := $(FBA_BURN_DIR) \
	$(FBA_BURN_DIR)/devices \
	$(FBA_BURN_DIR)/snd \
	$(CPS2_DIR) \
	$(FBA_BURN_DRIVERS_DIR)/cave \
	$(CPS3_DIR) \
	$(FBA_BURN_DRIVERS_DIR)/dataeast \
	$(GALAXIAN_DIR) \
	$(FBA_BURN_DRIVERS_DIR)/irem \
	$(FBA_BURN_DRIVERS_DIR)/konami \
	$(MD_DIR) \
	$(NEOGEO_DIR) \
	$(PCE_DIR) \
	$(PGM_DIR) \
	$(FBA_BURN_DRIVERS_DIR)/pre90s \
	$(FBA_BURN_DRIVERS_DIR)/psikyo \
	$(FBA_BURN_DRIVERS_DIR)/pst90s \
	$(FBA_BURN_DRIVERS_DIR)/sega \
	$(SNES_DIR) \
	$(FBA_BURN_DRIVERS_DIR)/taito \
	$(FBA_BURN_DRIVERS_DIR)/toaplan \
	$(FBA_BURN_DRIVERS_DIR)

FBA_CPU_DIRS := $(FBA_CPU_DIR) \
	$(FBA_CPU_DIR)/arm \
	$(FBA_CPU_DIR)/arm7 \
	$(FBA_CPU_DIR)/h6280 \
	$(FBA_CPU_DIR)/hd6309 \
	$(FBA_CPU_DIR)/i8039 \
	$(FBA_CPU_DIR)/konami \
	$(M68K_DIR) \
	$(FBA_CPU_DIR)/m6502 \
	$(FBA_CPU_DIR)/m6800 \
	$(FBA_CPU_DIR)/m6805 \
	$(FBA_CPU_DIR)/m6809 \
	$(FBA_CPU_DIR)/nec \
	$(FBA_CPU_DIR)/pic16c5x \
	$(FBA_CPU_DIR)/s2650 \
	$(FBA_CPU_DIR)/sh2 \
	$(FBA_CPU_DIR)/z80

FBA_LIB_DIRS := $(FBA_LIB_DIR)/zlib

FBA_SRC_DIRS := $(FBA_BURNER_DIR) $(FBA_BURN_DIRS) $(FBA_CPU_DIRS) $(FBA_BURNER_DIRS) $(FBA_LIBRETRO_DIRS)


ifeq ($(EXTERNAL_ZLIB), 1)
FBA_DEFINES += -DEXTERNAL_ZLIB
else
FBA_SRC_DIRS += $(FBA_LIB_DIRS)
endif

FBA_CXXSRCS := $(GRIFFIN_CXXSRCFILES) $(filter-out $(BURN_BLACKLIST),$(foreach dir,$(FBA_SRC_DIRS),$(wildcard $(dir)/*.cpp)))
FBA_CXXOBJ := $(FBA_CXXSRCS:.cpp=.o)
FBA_CSRCS := $(filter-out $(BURN_BLACKLIST),$(foreach dir,$(FBA_SRC_DIRS),$(wildcard $(dir)/*.c)))
FBA_COBJ := $(FBA_CSRCS:.c=.o)

OBJS := $(FBA_COBJ) $(FBA_CXXOBJ)

FBA_DEFINES := -DUSE_SPEEDHACKS -D__LIBRETRO__ \
	-D__LIBRETRO_OPTIMIZATIONS__ \
	-DWANT_NEOGEOCD \
	$(ENDIANNESS_DEFINES) \
	$(PLATFORM_DEFINES)
	
ifneq ($(platform),qnx)
   FBA_DEFINES += -DINLINE="static inline" -DSH2_INLINE="static inline"
endif

INCDIRS := -I$(FBA_BURNER_DIR)/win32 \
	-I$(LIBRETRO_DIR) \
	-I$(LIBRETRO_DIR)/tchar \
	-I$(FBA_BURN_DIR) \
	-I$(MAIN_FBA_DIR)/cpu \
	-I$(FBA_BURN_DIR)/snd \
	-I$(FBA_BURN_DIR)/devices \
	-I$(FBA_INTERFACE_DIR) \
	-I$(FBA_INTERFACE_DIR)/input \
	-I$(FBA_INTERFACE_DIR)/cd \
	-I$(FBA_BURNER_DIR) \
	-I$(FBA_CPU_DIR) \
	-I$(FBA_CPU_DIR)/i8039 \
	-I$(FBA_LIB_DIR)/zlib \
	-I$(FBA_BURN_DIR)/drv/capcom \
	-I$(FBA_BURN_DIR)/drv/dataeast \
	-I$(FBA_BURN_DIR)/drv/cave \
	-I$(FBA_BURN_DIR)/drv/neogeo \
	-I$(FBA_BURN_DIR)/drv/psikyo \
	-I$(FBA_BURN_DIR)/drv/sega \
	-I$(FBA_BURN_DIR)/drv/toaplan \
	-I$(FBA_BURN_DIR)/drv/taito \
	-I$(FBA_GENERATED_DIR) \
	-I$(FBA_LIB_DIR)

ifeq ($(LIBRETRO_OPTIMIZATIONS), 1)
FBA_DEFINES += -D__LIBRETRO_OPTIMIZATIONS__ 
endif

ifneq ($(platform), sncps3)
CFLAGS += -std=gnu99
endif

ifeq ($(DEBUG), 1)
CFLAGS += -O0 -g
CXXFLAGS += -O0 -g
else ifeq ($(platform), emscripten)
CFLAGS += -O2 -DNDEBUG
CXXFLAGS += -O2 -DNDEBUG
else
CFLAGS += -O3 -DNDEBUG
CXXFLAGS += -O3 -DNDEBUG
endif

ifeq ($(platform), sncps3)
WARNINGS_DEFINES =
else
WARNINGS_DEFINES = -Wno-write-strings
endif

CFLAGS += $(fpic) $(WARNINGS_DEFINES) $(FBA_DEFINES)
CXXFLAGS += $(fpic) $(WARNINGS_DEFINES) $(FBA_DEFINES)
LDFLAGS += $(fpic)

ifeq ($(FRONTEND_SUPPORTS_RGB565), 1)
CFLAGS += -DFRONTEND_SUPPORTS_RGB565
CXXFLAGS += -DFRONTEND_SUPPORTS_RGB565
endif

ifeq ($(ZLIB_INTERNAL), 0)
INCDIRS += -I$(FBA_LIB_DIR)
endif

ifeq ($(LIBRETRO_OPTIMIZATIONS), 1)
GENERATE_OPTS := -D__LIBRETRO_OPTIMIZATIONS__
else
GENERATE_OPTS :=
endif

PERL = perl$(EXE_EXT)
M68KMAKE_EXE = m68kmake$(EXE_EXT)
CTVMAKE_EXE = ctvmake$(EXE_EXT)
PGM_SPRITE_CREATE_EXE = pgmspritecreate$(EXE_EXT)
EXE_PREFIX = ./

generate-files-clean:
	rm -rf $(FBA_GENERATED_DIR)/
	rm -rf $(FBA_CPU_DIR)/m68k/m68kopac.c
	rm -rf $(FBA_CPU_DIR)/m68k/m68kopdm.c
	rm -rf $(FBA_CPU_DIR)/m68k/m68kopnz.c
	rm -rf $(FBA_CPU_DIR)/m68k/m68kops.c
	rm -rf $(FBA_CPU_DIR)/m68k/m68kops.h

generate-files:
	@mkdir -p $(FBA_GENERATED_DIR) 2>/dev/null || /bin/true
	@echo "Generating $(FBA_GENERATED_DIR)/driverlist.h..."
	@echo ""
	$(PERL) $(FBA_SCRIPTS_DIR)/gamelist.pl -o $(FBA_GENERATED_DIR)/driverlist.h -l gamelist.txt $(FBA_BURN_DRIVERS_DIR)/neogeo
	@echo ""
	@echo "Generating $(FBA_GENERATED_DIR)/neo_sprite_func.h..."
	@echo ""
	@echo "Generating $(FBA_GENERATED_DIR)/neo_sprite_func_table.h..."
	@echo ""
	$(PERL) $(FBA_SCRIPTS_DIR)/neo_sprite_func.pl -o $(FBA_GENERATED_DIR)/neo_sprite_func.h $(LIBRETRO_OPTIMIZATIONS)
	@echo ""

$(TARGET): $(OBJS)
	@echo "LD $@"
ifeq ($(STATIC_LINKING), 1)
	@$(AR) rcs $@ $(OBJS)
else
	@$(CXX) -o $@ $(SHARED) $(OBJS) $(LDFLAGS)
endif

%.o: %.cpp
	@echo "CXX $<"
	@$(CXX) -c -o $@ $< $(CXXFLAGS) $(INCDIRS)

%.o: %.c
	@echo "CC $<"
	@$(CC) -c -o $@ $< $(CFLAGS) $(INCDIRS)

clean-objs:
	rm -f $(OBJS)

clean:
	rm -f $(TARGET)
	rm -f $(OBJS)
	rm -f $(M68KMAKE_EXE)
	rm -f $(PGM_SPRITE_CREATE_EXE)
	rm -f $(CTVMAKE_EXE)
