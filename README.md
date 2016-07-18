ESS
===
<img style="float: right" src="https://github.com/sivaramambikasaran/ESS/raw/master/display.png" />

Extended SemiSeparable solver, which scales as O(N). The algorithm relies on embedding the semi-separable structure in an extended sparse matrix.

To run the solver key in the following:

	make -f makefile_ESS.mk

Then run the executable with the number of unknowns and rank of the semi-separability

	./ESS N m c

where `N` is the number of unknowns, `m` is the rank of semi-separability and `c` is a character to denote, if we want to compare with usual method or not. The semi-separable matrix obtained is a random one. To change the code, to get your desired semi-separable matrix, edit the file `testESS.cpp`. The lines you might need to change include the definition of the matrices `U`, `V` to obtain the semi-separability, vector of diagonal entries `d` and right hand side vector `rhs`.

[Dan](https://github.com/dfm) provides the python bindings, which also works for complex matrices (alpha and beta complex), and has kinldy made it available [here](https://github.com/dfm/ess).

Examples
--------
To solve a `5000` by `5000` dense system with semi-separable rank being `4` and to compare with the usual dense solver, key in

	./ESS 5000 4 y

To not compare with the dense solver, key in

	./ESS 500000 4 n

Typically, larger the value of `N` (say beyond `10000`), it is not advisable to compare with the usual solver, since the usual solver will take excruciatingly long time to solve.

____________
____________

GRP
===

The Generalized Rybicki Press algorithm can be interpreted as a special case of ESS. However, a naive ESS would lead to numerical issues (such as ill-conditioning, overflow/underflow) due to the presence of the exponential terms in the extended sparse matrix. The GRP circumvents this issue by a judicious analytic preconditioning (rescale the appropriate variables). To test that the GRP code is working, do the following:

	make -f makefile_GRP.mk

Then run the executable with the number of unknowns and rank of the semi-separability

	./GRP N m c

where `N` is the number of unknowns, `m` is the rank of semi-separability, i.e., the number of exponential sums in the covariance matrix and `c` is a character to denote, if we want to compare with usual method or not. The covariance matrix is of the form

	K(i,j) = sum_k a_kexp(-b_k|t_i-t_j|)	when i != j
	K(i,j) = d						 		when i = j

The timestamps `t_i`, `a_k`, `b_k`, `d` are chosen at random. To change the code, to get your desired covariance matrix, edit the file `testGRP.cpp`. The lines you might need to change include the definition of the vectors `alpha` (i.e., `a_k`'s), `beta` (i.e., `b_k`'s), `t` (i.e., the time stamps), the diagonal `d` and the right hand side `rhs`.

Examples
--------
To solve a `5000` by `5000` dense system with semi-separable rank being `4` and to compare with the usual dense solver, key in

	./GRP 5000 4 y

To not compare with the dense solver, key in

	./GRP 500000 4 n

Typically, larger the value of `N` (say beyond `10000`), it is not advisable to compare with the usual solver, since the usual solver will take excruciatingly long time to solve.
