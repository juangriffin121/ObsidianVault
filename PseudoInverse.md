$$pinv(A) = (A^TA)^{-1}$$
When i have an overdetermined system of equations:
$$A \vec x = \vec b$$
example:
	linear regression:
		A: two column matrix of all the x data and ones for the constant term
		$\vec x$: two component vector containing the slope and intercept
		$\vec b$:  column vector of all the y data 
		every row of A dotted with $\vec x$ is $mx + c = y$ 
		The only way this system is determined is with a square matrix which is the same as making a line with two points, or if multiple points exactly fall on the line, which is to say that the linear combination 