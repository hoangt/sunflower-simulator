TREEROOT	= ../../..
include $(TREEROOT)/conf/setup.conf

PROGRAM		= hello-trigraphs

PORT		= ../port
LIBOS		= mOS
TOOLSLIB	= $(TREEROOT)/tools/tools-lib
INCLUDES	= -I../port/  -I$(TREEROOT)/sys/include -I$(TREEROOT)/sys/kern/superH -I$(TREEROOT)/benchmarks/include -I$(TREEROOT)/sys/libOS/port
OPTFLAGS	= -gstabs3 -O0
CFLAGS		= -trigraphs $(TARGET-ARCH-FLAGS) -Wall # Do not do since we are linking mOS for libc implementation:	-nostdlib -fno-builtin 
LDFLAGS		= -Ttext $(LOADADDR) -TsuperH.ld -L$(TOOLSLIB)/$(TARGET) -L$(TREEROOT)/sys/libOS/$(LIBOS) -Map $(PROGRAM).map
LOADADDR	= 0x08004000


OBJS	=\
	init.o\
	$(PROGRAM).o\


all:	$(PROGRAM) $(PROGRAM).sr

$(PROGRAM): $(OBJS)
	$(LD) $(LDFLAGS) $(OBJS) -o $@ -lc -lgcc -lm -l$(LIBOS)-$(TARGET)

$(PROGRAM).sr:$(PROGRAM)
	$(OBJCOPY) -O srec $(PROGRAM) $@

$(PROGRAM).o: $(PROGRAM).c Makefile
	$(CC) $(CFLAGS) $(OPTFLAGS) $(INCLUDES) -c $(PROGRAM).c

init.o: init.S
	$(CPP) init.S > init.i; $(AS) init.i -o $@

clean:
	$(RM) init.i *.o $(PROGRAM) $(PROGRAM).sr $(PROGRAM).map

install: all
	cp $(PROGRAM).sr ../../../
