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
- Folder DissipativePDE contains tool for rigrous integration of PDEs:
  -  Folder DissipativePDE\Algebra contains the structure infinite series used in the algorithm and implemetation of operation for them,
  -  Folder DissipativePDE\Set contains the structure for sets which are used in the rigrous integration,
  -  Folder DissipativePDE\VectorField contains the structure for VectorField used in the rigrous integration,
  -  Folder DissipativePDE\VectorFieldMaker contains the additional methods which allow to produce the vector field fields for the Gallerking projection in the form of string used in CAPD IMap class,
  -  Folder DissipativePDE\SolverPDE contains methods and structs for rigrous C0 and C1 integration,
- Folder Utils contains mainly InPut OutPut settings
- Folder ChafeeInfante contains code dedicated specyfically for the Chafee-Infante:
   - Folder ChafeeInfante\ChafeeInfanteVecField contains  C0 and C1 vectorfields used in rigrous integration
   - Folder ChafeeInfante\GallerkinProjections contains the method to produce string, used in IMap class, for Gallerkin projection of the Chafee-Infante equation.

Sure! Here are the corrected grammar errors in your Markdown file:

CHAT GPT poprawione




# Chafee-Infante-ComputerAssistedProof

The code is used for computer-assisted proof of the existence of a periodic orbit and its attraction for the non-autonomous Chafee-Infante system:

$$u_t = u_{xx} + \lambda u - (A\sin(\omega t)+B)u^3,$$

where the solution $u(x,t):[0,\pi]\times[t_0,T]\to \mathbb{R}$ satisfies Dirichlet boundary codition, where $A,B\in\mathbb{R}$ and $\lambda,\omega\in \mathbb{R}$.

We work in the Fourier base, which means we write every point from the phase space in the form

$$ u^0 = \sum_{i=1}^\infty u_i^0\sin(ix).$$ 

The code contains 3 programs:

- `ChafeeInfante/sampleDyn`,
- `ChafeeInfante/findPeriodicPoint`,
- `ChafeeInfante/CAProof`.

The programs use data from files:
- `ChafeeInfante/textFiles/initialValue.txt` - It contains coefficients for sine odd series used as initial data for the programs. For example,

```r
{2., -0.5, 0.125, 0.3}
```

represents initial data in the form

$$
u^0 = 2\sin(x) - 0.5\sin(3x) - 0.125\sin(5x) + 0.3\sin(7x).
$$

- Parameters ${\lambda,\omega,A,B}$ are taken from file `ChafeeInfante/textFiles/params.txt` in the form 

```
{2, 6.28318530718, 1.5, 1}
-------------
{lambda, omega, A, B}
```

## ChafeeInfante/sampleDyn.cpp

Program for numerically integrating the Chafee-Infante system. 

- Integrating in the space of odd coefficients.
- The initial data is taken from the content of file `ChafeeInfante/textFiles/initialValue.txt`. The initial data determines the size of Gallerkin projection.
- File `ChafeeInfante/textFiles/sampleDynOptions.txt` contains options for the program:

```
0 1 2000
-------------
starting time, duration of simulation, number of steps
```

After running, the program should output sampled points from the trajectory and save them to file `ChafeeInfante/textFiles/sampleDynOutput.txt`.

## ChafeeInfante\findPeriodicPoint

Program for finding periodic orbits of the Chafee-Infate equation. It tries to find a fixed periodic point by iterating the Galerkin approximation of the map.

$$
T(u_0) = u(1,0;u_0).
$$

- The initial data for searching is taken from the content of file `ChafeeInfante\textFiles\initialValue.txt`. This initial data determines the size of the Galerkin projection.
- It assumes that parameter $\omega = 2\pi$, so the value of $\omega$ in file `ChafeeInfante\textFiles\params.txt` does not matter.
- It outputs the proposed approximation of the founded periodic orbit at zero. It also outputs an approximation of the variational matrix for the map $T$.


## ChafeeInfante\CAProof.cpp

Program for proving the existence of a periodic orbit and showing local attraction of this orbit. The program checks if some defined set $X_0$ holds the condition:

$$ T(X_0) \subset X_0 $$

If this condition is validated, then the existence of a periodic orbit is confirmed. For computation of the image, it uses a C0 rigorous algorithm for integration of PDEs. The program also tries to prove that the orbit is locally attracting by checking:

$$
||\frac{\partial T}{\partial x}(X_0)||_{C_0} < 1.
$$

The computation of derivatives is done using a C1 algorithm for rigorous integration.

- The set is centered around the initial condition $u^0$ from file `ChafeeInfante\textFiles\initialValue.txt`.
- File `ChafeeInfante\textFiles\sampleDynOptions.txt` contains options for setting up the computed assisted proof, such as:
```
1e-4 1 3 
6 14
9 17 0
-------------
eps , C, s
mainC0Size, fullC0Size
mainC1Size, fullC1Size, expColumns

```
- The set $X_0$ is defined as follows:
   $$X^0 = X_P^0 + X_Q^0,$$
  
with
$$X_P^0 = u^o(x)+ [-1,1]eps*\sum_{i=1}^n \sin((2i-1)x),$$

and

$$
    X_Q^o = \sum_{i=n+11}^\infty \frac{C[-11]}{(2i-11)^s}\sin((2i-11)x).
$$

where $n$ depends on the size of the initial condition.

- For C0 computation, we set:
   - `mainC0Size` - the number of modes taken for the differential inclusion,
   - `fullC0Size` - the number of modes represented explicitly.
- The rigorous C0 integration is done in the odd subspace of Fourier coefficients.

- For C1, we set:
   - `mainC1Size` -  the number of modes taken for the differential inclusion for each variable $(u,h)$ ,
   - `fullC1Size` -  the number of modes represented explicitly for each variable $(u,h)$ ,
   - `expColumns` - number of columns where derivatives are computed explicitly.

- The rigorous C1 integration is done in full space, not just in the odd subspace.
- In C1 integration, we have variables representing variational equations. Therefore, the actual number of modes used for differential inclusion is $2 \times \text{mainC1Size}$ and the number of modes represented explicitly is $2 \times \text{fullC1Size}$.
- The remaining columns are computed by setting them to be:

$$
    X^{o,h} = \sum_{i=\text{expColumns}+11}^\infty u_i\sin(ix),\quad \text{where } u_i\in[-11].
$$

## Code Information

- The programs are using the [CAPD library](http://capd.ii.uj.edu.pl/index.php) - a tool for nonrigorous and validated numerics for dynamical systems.
  - Used version of the library: 5.1.2,
  - The Makefile assumes that the CAPD library is located in the following position relative to the main directory:
   ```
   # directory where capd scripts are (e.g. capd-config)
   CAPDBINDIR = ../capd-capdDynSys-5.1.2/bin/
   
   ```

- Folder DissipativePDE contains tools for rigorous integration of PDEs:
  - Folder DissipativePDE\Algebra contains the structure infinite series used in the algorithm and implementation of operations on them,
  - Folder DissipativePDE\Set contains structures for sets which are used in rigorous integration,
  - Folder DissipativePDE\VectorField contains structures for VectorFields used in rigorous integration,
  - Folder DissipativePDE\VectorFieldMaker contains additional methods that allow producing vector field fields for Gallerkin projection in string form, which is used in CAPD IMap class,
  - Folder DissipativePDE\SolverPDE contains methods and structs for rigorous C0 and C1 integrations.

- Folder Utils mainly contains Input/Output settings.

- Folder ChafeeInfante contains code specifically dedicated to Chafee-Infante:
   - Folder ChafeeInfante\ChafeeInfanteVecField containing C0 and C1 vector fields used in rigrous integration
   - Folder ChafeeInfante\GallerkinProjections containing method to produce strings, used in IMap class, for Gallerkin projection of the Chafee-Infante equation.





  



