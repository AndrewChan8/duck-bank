all: bank

bank: bank.c
	gcc -g -o bank bank.c

clean:
	rm -f *.o bank output.txt ledger.txt

check:
	diff output.txt outout.txt