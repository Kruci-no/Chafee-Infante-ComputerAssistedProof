# Chafee-Infante-ComputerAssistedProof
The code is used for computer assisted proof of existence of periodic orbit and its atraction for the non-autonomous Chafee-Infante system:
   $$u_t = u_{xx} + \lambda u - (A\sin(\omega t)+B)u^3,$$
    where solution $u(x,t):[0,\pi]\times[t_0,T]\to \mathbb{R}$ satisfies Dirichlet boundary solutions, where $A,B\in\mathbb{R}$ and $\lambda,\omega\in \mathbb{R}.$
We work in the Fourier base that is we write every point from the phase space in the form

$$ u^0 = \sum_{i=1}^\infty u_i^0\sin(ix).$$ 

Code contains 3 programs:
- ChafeeInfante\sampleDyn,
- ChafeeInfante\findPeriodicPoint,
- ChafeeInfante\CAProof.

The programs uses data from files:
- ChafeeInfante\textFiles\initialValue.txt - It contains coefficients for sin odd series used as initial data of for the programs: for example
```r
{2.,-0.5, 0.125,0.3} - Format of initial data
```
would represent initial data in the form

$$
u^0 = 2\sin(x)  -0.5\sin(3x) - 0.125\sin(5x)+ 0.3\sin(7x).
$$

- Parameters ${\lambda,\omega,A,B}$ are taken from file ChafeeInfante\textFiles\params.txt in the form 
 ```
{2,6.28318530718,1.5,1}
-------------
{lambda,omega,A,B}
```
## ChafeeInfante\sampleDyn.cpp
Program for numerical integrating the Chafee-Infante system program. </br>
- Integrating in the space of odd coefficients.
- As initial data is taken the content od file ChafeeInfante\textFiles\initialValue.txt - initial data determines the size of Gallerkin projection.  
- File ChafeeInfante\textFiles\sampleDynOptions.txt contains options for the program: 
```
0 1 2000 
-------------
starting time, duration of symulation, number of steps
```
- After runing program should output sampled point from the trajectory of initial is saved to file ChafeeInfante\textFiles\sampleDynOutPut.txt

## ChafeeInfante\findPeriodicPoint
Program for finding periodic orbit of the Chafee-Infate equation . It try to find fixed periodic point by iterating the Gallerkin aproximation of the map

$$
T(u_0) = u(1,0;u_0).
$$
- As initial data for searching initial data is taken the content od file ChafeeInfante\textFiles\initialValue.txt - initial data determines the size of Gallerkin projection,
- It assumes that parameter $\omega = 2\pi$ so the value $\omega$ in file ChafeeInfante\textFiles\params.txt does not matter,
- It outputing the proposed aproximation of aproximation of founded periodic orbit in zero in the. It also output the aproksimation of the variational matrix for the map $T$.

 ## ChafeeInfante\CAProof.cpp
 Program for proving the existence of periodic orbit and showing local attraction of this orbit.
 Program cheaks if some defined set $X_0$ there holds
 
$$ T(X_0)\subset X_0$$

if the condition is validated the existence is of periodic orbit is validated. For computation of image it is using C0 rigrous algorithm of integration of pdes. Then program is trying to prove that it is locally attracting.  Namelly it is cheaking if:

$$
||\frac{\partial T}{\partial x}(X_0)||_{C_0}< 1.
$$

The computation of derivative is done by C1 algorithm of rigrous integration.
- The set is centered arround the initial condtion $u^0$ form file ChafeeInfante\textFiles\initialValue.txt,
- File ChafeeInfante\textFiles\sampleDynOptions.txt contains options for the setting of the computed assisted proof for example:
```
1e-4 1 3 
6 14
9 17 0
-------------
eps , C, s
mainC0Size, fullC0Size
mainC1Size, fullC1Size, expColumns

```
- The set $X_0$ is defined by the following way
  $$X^0 = X_P^0 + X_Q^0,$$
  
with
$$X_P^0 =u^0(x)+ [-1,1]eps*\sum_{i=1}^n  \sin((2i-1)x),$$

and

$$
    X_Q^0 = \sum_{i=n+1}^\infty\frac{C[-1,1]}{(2i-1)^s} \sin((2i-1) x).
$$

where $n$ depends on the size of initial condtiodin.

- For the C0 computation we set:
   - mainC0Size -  the number of modes taken for the differential inclusion,
   - fullC0Size - the number of modes represented exsplicitly.
- The rigrous C0 integration is done in odd subspace of Fourier coefficients.

- For the C1 we set:
   - mainC1Size -  the number of modes taken for the differential inclusion for each variable $(u,h)$ ,
   - fullC1Size - the number of modes represented exsplicitly for each variable $(u,h)$ ,
   - expColumns - number of columns of the derivative computed explicitly.

- The rigrous C1 integration is done in full space not in the Odd subspace.
- For C1 integration we have variables the variational eqation, so the real number of modes takes for differetial incusion is 2*mainC1Size and the number of modes represented explicitly is equal to 2*fullC1Size.
- Rest of the columns are computed by commansing them to the one set  the form

$$
    X^{0,h} = \sum_{i=expColumns+1}^\infty u_i \sin(ix),\quad \text{where }u_i\in[-1,1].
$$

 ## The code informations
- The programs are using the [CAPD library](http://capd.ii.uj.edu.pl/index.php) - Tool for nonrigorous and validated numerics for dynamical systems.
  - Used version of library 5.1.2,
  - Makefile assumes that the CAPD library is in the following position with respect to main directiory,
   ```
   # directory where capd scripts are (e.g. capd-config)
   CAPDBINDIR = ../capd-capdDynSys-5.1.2/bin/
   
   ```
- Folder DissipativePDE contains tool for rigrous integration of PDEs
  -  Folder DissipativePDE\Algebra contains the structure infinite series used in the algorithm and implemetation of operation for them
  -  Folder DissipativePDE\Set contains the structure for sets which are used in the rigrous integration.
  -  Folder DissipativePDE\VectorField contains the structure for VectorField used in the rigrous integration.
  -  Folder DissipativePDE\VectorFieldMaker contains the additional methods which allow to produce the vector field fields for the Gallerking projection in the form of string used in CAPD IMap class.
  -  Folder DissipativePDE\SolverPDE contains methods and structs for rigrous C0 and C1 integration. 




  



