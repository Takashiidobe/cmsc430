UNAME := $(shell uname)

ifeq ($(UNAME), Darwin)
  format=macho64
else
  format=elf64
endif

objs = \
    main.o \
    print.o

default: runtime.o

runtime.o: $(objs)
	ld -r $(objs) -o runtime.o

%.run: %.o runtime.o
	gcc runtime.o $< -o $@

.c.o:
	gcc -fPIC -c -g -o $@ $<

.s.o:
	nasm -g -f $(format) -o $@ $<

%.s: %.rkt
	racket -t compile-file.rkt -m $< > $@

clean:
	rm *.o *.s *.run

%.test: %.run %.rkt
	@test "$(shell ./$(<))" = "$(shell racket $(word 2,$^))"
