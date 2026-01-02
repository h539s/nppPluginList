# Makefile for nppPluginList using LLVM MinGW

# Default architecture
ARCH ?= x64

# Build directory
BUILD_DIR := build$(ARCH)

# Set tools and flags based on architecture
ifeq ($(ARCH), Win32)
    CXX := clang++
    RC := llvm-windres
    TARGET_DIR := ../bin
    TARGET := $(TARGET_DIR)/nppPluginList.dll
    DEFINES := -DWIN32 -DNDEBUG -D_WINDOWS -D_USRDLL -DNPPPLUGINLIST_EXPORTS
    RC_DEFINES := -D_WIN32 -D_UNICODE -DUNICODE
	TARGET_ARCH := i686-w64-mingw32
else ifeq ($(ARCH), x64)
    CXX := clang++
    RC := llvm-windres
    TARGET_DIR := ../bin64
    TARGET := $(TARGET_DIR)/nppPluginList.dll
    DEFINES := -DNDEBUG -D_WINDOWS -D_USRDLL -DNPPPLUGINLIST_EXPORTS
    RC_DEFINES := -D_WIN64 -D_UNICODE -DUNICODE
	TARGET_ARCH := x86_64-w64-mingw32
else ifeq ($(ARCH), Arm64)
    CXX := clang++
    RC := llvm-windres
    TARGET_DIR := ../binarm64
    TARGET := $(TARGET_DIR)/nppPluginList.dll
    DEFINES := -DNDEBUG -D_WINDOWS -D_USRDLL -DNPPPLUGINLIST_EXPORTS
    RC_DEFINES := -D_WIN64 -D_M_ARM64 -D_UNICODE -DUNICODE
	TARGET_ARCH := aarch64-w64-mingw32
else
    $(error Unsupported architecture: $(ARCH). Use Win32, x64, or Arm64)
endif

# Common flags
CXXFLAGS := -Wall -Wextra -O2 -s --target=$(TARGET_ARCH) -stdlib=libc++ -fuse-ld=lld $(DEFINES)
LDFLAGS := -shared --target=$(TARGET_ARCH) -fuse-ld=lld -static

# Source files
SRCS := ../src/dllmain.cpp
RC_SRCS := ../src/nppPluginList.rc

# Object files (place them in the build directory)
OBJS := $(patsubst ../src/%.cpp,$(BUILD_DIR)/%.o,$(SRCS))
RC_OBJS := $(patsubst ../src/%.rc,$(BUILD_DIR)/%.o,$(RC_SRCS))

.PHONY: all clean

all: $(TARGET)

$(TARGET): $(OBJS) $(RC_OBJS)
	if not exist "$(TARGET_DIR)" mkdir "$(TARGET_DIR)"
	$(CXX) $(OBJS) $(RC_OBJS) $(LDFLAGS) -o $@

# Rule to compile C++ source files into the build directory
$(BUILD_DIR)/%.o: ../src/%.cpp
	if not exist "$(@D)" mkdir "$(@D)"
	$(CXX) --target=$(TARGET_ARCH) $(CXXFLAGS) -c $< -o $@

# Rule to compile resource files into the build directory
$(BUILD_DIR)/%.o: ../src/%.rc
	if not exist "$(@D)" mkdir "$(@D)"
	$(RC) --target=$(TARGET_ARCH) $(RC_DEFINES) -i $< -o $@

clean:
	rm -rf $(BUILD_DIR)
	rm -f ../bin/nppPluginList.dll ../bin64/nppPluginList.dll ../binarm64/nppPluginList.dll
