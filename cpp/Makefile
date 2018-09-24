# Build toolchain
PKGCONFIG = i686-w64-mingw32-pkg-config
CC = i686-w64-mingw32-g++
AR = i686-w64-mingw32-ar

# Common flags
CFLAGS = -c -Wall -std=c++14 -g -I./include -DUNICODE
# Convenience defs for linking DirectPlay/dplib
LD_DPLAY = -ldxguid -ldplayx
LD_DPLIB = -Llib/ -ldplib $(LD_DPLAY)

# dplib target
DPLIB_SRC = src
DPLIB_FILES = $(shell find $(DPLIB_SRC) -name "*.cpp" \! -path "*/test/*")
DPLIB_OBJECTS = $(DPLIB_FILES:.cpp=.o)
DPLIB_TARGET = lib/dplib.lib

# Tests
TEST_SRC = test
TEST_FILES = $(shell find $(TEST_SRC) -name "*.cpp")
TEST_OBJECTS = $(TEST_FILES:.cpp=.o)
TEST_TARGET = test.exe

# All objects (used for cleanup)
OBJECTS = \
	$(DPLIB_OBJECTS) \
	$(TEST_OBJECTS)

all: $(DPLIB_TARGET)

# Library stuffs

lib:
	mkdir lib

$(DPLIB_TARGET): lib $(DPLIB_OBJECTS)
	$(AR) rvs $@ $(DPLIB_OBJECTS)

# Compiling

$(DPLIB_SRC)/%.o: $(DPLIB_SRC)/%.cpp
	$(CC) $(CFLAGS) $< -o $@

# Tests

$(TEST_TARGET): $(DPLIB_TARGET) $(TEST_OBJECTS)
	$(CC) -o $@ $(TEST_OBJECTS) $(TEST_LDFLAGS)

test: $(TEST_TARGET)
	wine $(TEST_TARGET)

# Cleanups

clean:
	rm -f $(OBJECTS) $(DPLIB_TARGET) $(TEST_TARGET)

.PHONY: all run clean
