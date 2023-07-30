# Chafee-Infante-ComputerAssistedProof
The code is used for computer assisted proof of existence of periodic orbit and its atraction for the non-autonomous Chafee-Infante system:
   $$u_t = u_{xx} + \lambda u - (A\sin(\omega t)+B)u^3,$$
    where solution $u(x,t):[0,\pi]\times[t_0,T]\to \mathbb{R}$ satisfies Dirichlet boundary solutions, where $A,B\in\mathbb{R}$ and $\lambda,\omega\in \mathbb{R}.$
We work in the Fourier base that is we write every point from the phase space in the form

$$ u = \sum_{i=1}^\infty u_i\sin(ix).$$ 

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
u_0 = 2\sin(x)  -0.5\sin(3x) - 0.125\sin(5x)+ 0.3\sin(7x)
$$
- Parameters ${\lambda,\omega,A,B}$ are taken from file ChafeeInfante\textFiles\params.txt in the form 
 ```
{2,6.28318530718,1.5,1}
{lambda,omega,A,B}
```
## ChafeeInfante\sampleDyn.cpp
Program for numerical integrating the Chafee-Infante system program. </br>
- Integrating in the space of odd coefficients.
- As initial data is taken the content od file ChafeeInfante\textFiles\initialValue.txt - initial data determines the size of Gallerkin projection.  
- File ChafeeInfante\textFiles\sampleDynOptions.txt contains options for the program: 
```
0 1 2000 
Optionas for: starting time, duration of symulation, number of steps
```
- After runing program should output sampled point from the trajectory of initial is saved to file ChafeeInfante\textFiles\sampleDynOutPut.txt

## ChafeeInfante\findPeriodicPoint
Program for finding periodic orbit of the Chafee-Infate equation. It try to find fixed periodic point by iterating the Gallerkin aproximation of the map

$$
T(u_0) = u(1,0;u_0).
$$
- As initial data for searching initial data is taken the content od file ChafeeInfante\textFiles\initialValue.txt - initial data determines the size of Gallerkin projection,
- It assumes that parameter $\omega = 2\pi$ so the value $\omega$ in file ChafeeInfante\textFiles\params.txt does not matter,
- It outputing the proposed aproximation of aproximation of founded periodic orbit in zero in the. It also output the aproksimation of the variational matrix for the map $T$.

 ## ChafeeInfante\CAProof.cpp
 Program for proving the existence of periodic orbit and showing local attraction of this orbit.
 Program cheaks if some set $X_0$ there holds 
$$ T(X_0)\subset X_0$$
if so 
