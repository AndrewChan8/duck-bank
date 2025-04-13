all: bank

bank: bank.c
	gcc -g -o bank bank.c

clean:
	rm -rf *.o bank output.txt ledger.txt output

check:
	diff output.txt outout.txt