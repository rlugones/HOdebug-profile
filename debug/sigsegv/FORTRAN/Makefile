all: small big

small:
	gfortran source.f90 my_func.f -o small.x -D__SMALL -O0 -g

big:
	gfortran source.f90 my_func.f -o big.x -O0 -g

clean:
	rm *.x
