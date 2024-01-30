SHELL := /bin/bash
CC=gcc
CFLAGS =-Wall -Wextra -Werror
EXEC = s21_cat
FILE =cat_test.txt
SRC = cat.c
CHECK= 2>&1 valgrind --tool=memcheck --leak-check=yes 
OBJ = $(SRC:.c=.o)

all: $(EXEC)
$(EXEC): $(OBJ)
	$(CC) $(CFLAGS) $(OBJ) -o $(EXEC)
	rm -f *.o

tests1:
	-diff <(./s21_cat $(FILE)) <(cat $(FILE))
	-diff <(./s21_cat -e $(FILE)) <(cat -e $(FILE))
	-diff <(./s21_cat -E $(FILE)) <(cat -E $(FILE))
	-diff <(./s21_cat -t $(FILE)) <(cat -t $(FILE))
	-diff <(./s21_cat -T $(FILE)) <(cat -T $(FILE))
	-diff <(./s21_cat -s $(FILE)) <(cat -s $(FILE))
	-diff <(./s21_cat --squeeze-blank $(FILE)) <(cat -s $(FILE))
	-diff <(./s21_cat -n $(FILE)) <(cat -n $(FILE))
	-diff <(./s21_cat --number $(FILE)) <(cat -n $(FILE))
	-diff <(./s21_cat -b $(FILE)) <(cat -b $(FILE))
	-diff <(./s21_cat --number-nonblank $(FILE)) <(cat -b $(FILE))

tests2:
	$(CHECK) ./s21_cat $(FILE) | grep ERROR
	$(CHECK) ./s21_cat -n $(FILE) | grep ERROR
	$(CHECK) ./s21_cat -e $(FILE) | grep ERROR
	$(CHECK) ./s21_cat -t $(FILE) | grep ERROR
	$(CHECK) ./s21_cat -b $(FILE) | grep ERROR
	$(CHECK) ./s21_cat -s $(FILE) | grep ERROR
	$(CHECK) ./s21_cat -etsnb $(FILE) | grep ERROR

%.o: %.c
	$(CC) $(CFLAGS) -c $< -o $@

clean:
	rm -f $(OBJ) $(EXEC)

rebuild: clean $(EXEC)

cppcheck:
	cppcheck --enable=all --suppress=missingIncludeSystem *.c *.h ./../common/*.*

.PHONY: format
format:
	clang-format -i *.c *.h