Download Link: https://assignmentchef.com/product/solved-cuda-homework4-cuda-programming
<br>
The purpose of this assignment is to familiarize yourself with CUDA programming.

In this problem, you need to use CUDA to parallelize concurrent wave equation (<a href="https://en.wikipedia.org/wiki/Wave_equation">http://en.wikipedia. </a><a href="https://en.wikipedia.org/wiki/Wave_equation">org/wiki/Wave</a> <a href="https://en.wikipedia.org/wiki/Wave_equation">equation</a>). Below show a serial implementation of the concurrent wave equation (<a href="http://www.cs.nctu.edu.tw/~ypyou/courses/PP-f19/assignments/HW4/serial_wave.c">http: </a><a href="http://www.cs.nctu.edu.tw/~ypyou/courses/PP-f19/assignments/HW4/serial_wave.c">//www.cs.nctu.edu.tw/</a><a href="http://www.cs.nctu.edu.tw/~ypyou/courses/PP-f19/assignments/HW4/serial_wave.c"><sub>∼</sub></a><a href="http://www.cs.nctu.edu.tw/~ypyou/courses/PP-f19/assignments/HW4/serial_wave.c">ypyou/courses/PP-f19/assignments/HW4/serial</a> <a href="http://www.cs.nctu.edu.tw/~ypyou/courses/PP-f19/assignments/HW4/serial_wave.c">wave.c</a>).

<table width="610">

 <tbody>

  <tr>

   <td width="610">/********************************************************************** * DESCRIPTION:*           Serial Concurrent Wave Equation – C Version*           This program implements the concurrent wave equation*********************************************************************/#include &lt;stdio.h&gt;#include &lt;stdlib.h&gt;#include &lt;math.h&gt;#include &lt;time.h&gt;#define MAXPOINTS 1000000#define MAXSTEPS 1000000#define MINPOINTS 20 #define PI 3.14159265void check_param(void); void init_line(void); void update (void); void printfinal (void);int nsteps,            /* number of time steps */ tpoints,  /* total points along string */ rcode; /* generic return code */float values[MAXPOINTS+2],        /* values at time t */ oldval[MAXPOINTS+2],            /* values at time (t-dt) */ newval[MAXPOINTS+2];  /* values at time (t+dt) *//********************************************************************** *              Checks input values from parameters*********************************************************************/ void check_param(void){char tchar[20];/* check number of points, number of iterations */ while ((tpoints &lt; MINPOINTS) || (tpoints &gt; MAXPOINTS)) { printf(“Enter number of points along vibrating string [%d-%d]: ”,MINPOINTS, MAXPOINTS); scanf(“%s”, tchar); tpoints = atoi(tchar);if ((tpoints &lt; MINPOINTS) || (tpoints &gt; MAXPOINTS)) printf(“Invalid. Please enter value between %d and %d
”, MINPOINTS, MAXPOINTS);}while ((nsteps &lt; 1) || (nsteps &gt; MAXSTEPS)) { printf(“Enter number of time steps [1-%d]: “, MAXSTEPS); scanf(“%s”, tchar);</td>

  </tr>

 </tbody>

</table>

nsteps = atoi(tchar); if ((nsteps &lt; 1) || (nsteps &gt; MAXSTEPS)) printf(“Invalid. Please enter value between 1 and %d
”, MAXSTEPS);

}

printf(“Using points = %d, steps = %d
”, tpoints, nsteps);

}

/**********************************************************************

*                            Initialize points on line

*********************************************************************/ void init_line(void)

{

int i, j; float x, fac, k, tmp;

/* Calculate initial values based on sine curve */ fac = 2.0 * PI; k = 0.0; tmp = tpoints – 1; for (j = 1; j &lt;= tpoints; j++) { x = k/tmp; values[j] = sin (fac * x); k = k + 1.0;

}

/* Initialize old values array */ for (i = 1; i &lt;= tpoints; i++) oldval[i] = values[i];

}

/********************************************************************** *          Calculate new values using wave equation

*********************************************************************/ void do_math(int i)

{

float dtime, c, dx, tau, sqtau;

dtime = 0.3; c = 1.0; dx = 1.0; tau = (c * dtime / dx); sqtau = tau * tau;

newval[i] = (2.0 * values[i]) – oldval[i] + (sqtau * (-2.0)*values[ i]);

}

/********************************************************************** *          Update all values along line a specified number of times

*********************************************************************/ void update()

{

int i, j;

/* Update values for each time step */ for (i = 1; i&lt;= nsteps; i++) {

<table width="610">

 <tbody>

  <tr>

   <td width="610">/* Update points along line for this time step */ for (j = 1; j &lt;= tpoints; j++) { /* global endpoints */ if ((j == 1) || (j == tpoints)) newval[j] = 0.0;else do_math(j);}/* Update old values with new values */ for (j = 1; j &lt;= tpoints; j++) { oldval[j] = values[j]; values[j] = newval[j];}}}/***********************************************************************                         Print final results*********************************************************************/ void printfinal(){int i;for (i = 1; i &lt;= tpoints; i++) { printf(“%6.4f “, values[i]); if (i%10 == 0)printf(“
”);}}/***********************************************************************                     Main program*********************************************************************/ int main(int argc, char *argv[]){ sscanf(argv[1],”%d”,&amp;tpoints); sscanf(argv[2],”%d”,&amp;nsteps); check_param();printf(“Initializing points on the line…
”); init_line();printf(“Updating all points for all time steps…
”); update();printf(“Printing final results…
”); printfinal(); printf(“
Done.

”);return 0;}</td>

  </tr>

 </tbody>

</table>

<h1>2           Requirements</h1>

<ul>

 <li>Your program should take two command-line arguments, which indicate the number of points andthe number of iterations, respectively.</li>

 <li>The output format should not be changed.</li>

</ul>

<h1>3           Development Environment</h1>

<h2>3.1          Building the CUDA environment on your own computer</h2>

If you have an nVIDIA GPU, you can build your own development environment by installing CUDA SDK.

<a href="https://developer.nvidia.com/cuda-downloads">https://developer.nvidia.com/cuda-downloads</a>

<h2>3.2          Using NCTU CS CUDA Server</h2>

We have set up four servers for this assignment. You can login to one of the servers to work on your assignment. Each server contains a GPU. TAs will grade your implementation on these servers, so please make sure your implementation works on the provided servers.

<h3>3.2.1         SSH Login Information</h3>

<table width="400">

 <tbody>

  <tr>

   <td width="107">IP</td>

   <td width="89">Port</td>

   <td width="88">User Name</td>

   <td width="117">Password</td>

  </tr>

  <tr>

   <td width="107">140.113.215.195</td>

   <td width="89">37031–37034</td>

   <td width="88">[Student ID]</td>

   <td width="117">[Provided by TA]</td>

  </tr>

 </tbody>

</table>

<h3>3.2.2         Compilation</h3>

You can use gcc to compile serial wave.c gcc serial wave.c -o serial wave -lm

Use nvcc to compile cuda wave.cu. nvcc cuda wave.cu -o cuda wave