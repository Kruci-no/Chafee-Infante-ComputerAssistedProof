# Chafee-Infante-ComputerAssistedProof
    The code is used for computer assisted proof of existence of periodic orbit and its atraction for the non-autonomous Chafee-Infante system:
     $$u_t = u_{xx} + \lambda u - (A+B\sin(\omega t))u^3,$$
    where solution $u(x,t):[0,\pi]\times[t_0,T]\to \mathbb{R}$ satisfies Dirichlet boundary solutions, where $A,B\in\mathbb{R}$ and $\lambda,\omega\in \mathbb{R}.$
    We work in the Fourier base that is we write every point from the phase space in the form
    $$ u = \sum_{i=1}^\infty u_i\sin(ix).$$ 
Code contains 3 program
- ChafeeInfante\sampleDyn.cpp - Program for numerical integrating the Chafee-Infante system program:
        - It is integrating in the space of odd coefficients 
        - It used as initial data the content od file ChafeeInfante\textFiles\initialValue.txt - initial data determines the size of Gallerkin projection. 
        in the form {$\lambda,\omega,A,B$} 
        - Parameters of integration are taken from file ChafeeInfante\textFiles\params.txt, 
        - File ChafeeInfante\textFiles\sampleDynOptions.txt contains options for the program: starting time, duration of symulation, number of steps
