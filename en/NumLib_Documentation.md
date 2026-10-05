# NumLib Documentation

## Contents

  * 1 Introduction
    * 1.1 Naming convention of routines
  * 2 Data types and declarations (unit _typ_)
    * 2.1 Basic floating point type, ArbFloat
    * 2.2 Basic integer type, ArbInt
    * 2.3 Vectors
    * 2.4 Matrices
      * 2.4.1 Generic matrices
      * 2.4.2 Symmetric matrices
      * 2.4.3 Symmetric positive definite matrices
      * 2.4.4 Band matrices
      * 2.4.5 Symmetric band matrices
      * 2.4.6 Tridiagonal matrices
      * 2.4.7 Symmetric tridiagonal matrices
    * 2.5 Complex numbers
  * 3 Basic operations with matrices and vectors (unit _omv_)
    * 3.1 Inner product of two vectors
    * 3.2 Product of two matrices
    * 3.3 Product of a matrix with a vector
    * 3.4 Transpose matrix
    * 3.5 Norm of vectors and matrices
  * 4 Determinant of a matrix (unit _det_)
    * 4.1 Determinant of a standard matrix
    * 4.2 Determinant of a band matrix
    * 4.3 Determinant of a symmetric positive definite band matrix
    * 4.4 Determinant of a tridiagonal matrix
  * 5 Inverse of a matrix (unit _inv_)
  * 6 Solving systems of linear equations (unit _sle_)
    * 6.1 Square matrices with standard storage
    * 6.2 Band matrix
    * 6.3 Symmetric positive definite band matrix
    * 6.4 Tridiagonal matrix
    * 6.5 Overdetermined systems (least squares)
  * 7 Eigenvalues and eigenvectors (unit _eig_)
    * 7.1 Matrices with general storage
    * 7.2 Symmetric band matrices
    * 7.3 Symmetric tridiagonal matrices
  * 8 Finding the roots of a function (unit _roo_)
    * 8.1 Roots of a polynomial
    * 8.2 Roots of the quadratic equation
    * 8.3 Roots of the binomial equation
    * 8.4 Bisection
    * 8.5 Roots of a system of nonlinear equations
  * 9 Numerical integration of a function (unit _int_)
  * 10 Ordinary differential equations (unit _ode_)
    * 10.1 Solving a single first-order differential equation
    * 10.2 Solving a system of first-order differential equations
  * 11 Interpolation and fitting (unit _ipf_)
    * 11.1 Fitting with a polynomial
    * 11.2 Interpolation with a natural cubic spline
    * 11.3 Fitting with a natural cubic spline
  * 12 Special functions (unit _spe_)
    * 12.1 Evaluation of a polynomial
    * 12.2 Error function
    * 12.3 Normal distribution
    * 12.4 Gamma function
    * 12.5 Incomplete gamma function
    * 12.6 Beta function
    * 12.7 Incomplete beta function
    * 12.8 Bessel functions
    * 12.9 Other functions



## Introduction

NumLib is a collection of routines for numerical mathematics. It was written by C.J.J.M. van Ginneken, W.J.P.M. Kortsmit, and L. Reij at the Technical University of Eindhoven (Netherlands) and donated to the Free Pascal project. Its beginnings date back to 1965. Originally written in Algol 60, ported to poorly formatted Pascal with strange procedure names, it is hard stuff, in particular because an official documentation is not readily available (well... -- Dutch documentation files in TeX can be found at <http://www.stack.nl/~marcov/numlib/>). 

But after all, this library contains a series of very useful routines grouped into several units: 

  * Calculation of special functions (unit _spe_)
  * Solution of systems of linear equations (unit _sle_)
  * Finding roots of equations (unit _roo_)
  * Fitting routines (unit _ipf_)
  * Basic operations with matrices and vectors (unit _omv_)
  * Matrix inversion (unit _inv_)
  * Calculation of determinants (unit _det_)
  * Calculation of eigenvalues (unit _eig_)



Therefore, it is the intention of this wiki article to create some useful documentation. Each routine described here was tested and its description will be accompanied by a working code example. 

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** This page heavily makes use of mathematical formulas written in LaTeX. In order to see these formulas in the browser JavaScript must be enabled. 

### Naming convention of routines

Would you imagine that a procedure named `spears` calculates the arcsin? The original NumLib routines limit the procedure/function names to 6 characters (maybe a limitation of Algol?). The first three characters always are identical with the name of the unit in which they are implemented. This leaves only three characters to differentiate between the routines... 

Routines later added to the original library are given more conventional names, such as `normaldist` for calculation of the normal distribution. 

## Data types and declarations (unit _typ_)

### Basic floating point type, ArbFloat

In NumLib, a new type `ArbFloat` is defined which is used as floating point type throughout the entire library. This allows to switch between IEEE Double (64bit) and Extended(80bit) (though big endian state is unknown at present). 

At the time of this writing the `$DEFINE ArbExtended` is enabled, and therefore `ArbFloat` is identical to the standard type `extended`. In order to change the floating point type define or undefine `ArbExtended` and adapt the machineconstants to the type selected. 

  


### Basic integer type, ArbInt

Because in plain FPC mode the type Integer is 16-bits (for TP compatibility), and in Delphi 32-bits, all integers were changed to `ArbInt`. The basic idea is the same as with `ArbFloat`. 

  


### Vectors

NumLib often passes **vectors** to procedures as one `ArbFloat` plus some integer value. This `ArbFloat` value is the first value of an array containing the vector, the integer is the count of elements used from this array. Internally an array type is mapped over the first value in order to access the other array elements. This way both 0- and 1-based arrays as well as static and dynamic arrays can be passed to the functions and procedures of NumLib. 
    
    
    procedure DoSomething(var AVector: ArbFloat; n: Integer);
    type
      TLocalVector = array[0..MaxElements] of ArbFloat;
    var
      i: Integer;
      x: TLocalVector absolute AVector;
    begin
      for i:=0 to n-1 do
        // do something with x[i]
    end;
    
    var
      x: array[1..100];   // Note: This array begins with index 1!
    ...
      DoSomething(x[1], 100);
    

  


### Matrices

A matrix is a two-dimensional arrangement of numbers in _m_ rows and _n_ columns. 

    [math]\displaystyle{ {A} = \left[ \begin{array}{cccc} a_{11} & a_{12} & a_{13} & \ldots & a_{1n}\\\ a_{21} & a_{22} & a_{23} & \ldots & a_{2n}\\\ \vdots & \vdots & \vdots & \ddots & \vdots\\\ a_{m1} & a_{m2} & a_{m3} & \ldots & a_{mn} \end{array} \right] }[/math]

The matrix elements _a ij_ are addressed by row and column indexes, _i_ and _j_ , respectively. In mathematics, the index values usually start with 1 (_i_ = 1.._m_ , _j_ = 1.._n_) while in computing the first index is preferred to be 0 (_i_ = 0..._m_ -1, _j_ = 0..._n_ -1). 

#### Generic matrices

In this type of matrix, no assumption is made on symmetry and shape of the data arrangement. 

A constant (real) matrix can be declared in Pascal as a two-dimensional array of floating point numbers: `A: array[1..m, 1..n] of ArbFloat` (or `A: array[0..m-1, 0..n-1] of ArbFloat` in 0-based index notation). Similarly, a constant complex matrix can be declared as an `array[1..m, 1..n] of complex`. 

When a NumLib procedure needs a matrix the first (top left) matrix element must be passed as a var parameter, along with the number of rows and columns. It is even possible to allocate larger arrays as needed, and in this case the allocated row length (or number of columns) must be specified. 

Alternatively a matrix also can be specified as a one-dimensional array. This works because all data are found within the same memory block. The array index, `k` must be converted to the 2D matrix indexes, `i, j`: 

` `

`
    
    
     k := i * n + j      <==> i = k div n,           j := k mod n          {for 0-based array and matrix indexes}
     k := (i-1) * n + j  <==> i := (k-1) div n + 1,  j := (k-1) mod n + 1  {for 1-based array and matrix indexes}
    

```

``

Using a one-dimensional array is also the correct way to dynamically allocate memory for a matrix. The first idea of using an `array of array of ArbFloat` will not work because this does not allocate a single memory block. Instead, individual memory blocks will be allocated for the inner array (rows), and these pointers will be stored in the outer array - this way of storing 2D data is not compatible with the requirements of NumLib. 

Here is a simple example how to create, populate and read a matrix dynamically 

    [math]\displaystyle{ A = \left[ \begin{array}{ccc} 1 & 2 & 3 \\\ 4 & 5 & 6 \end{array} \right] }[/math]
    
    
    var
      A: array of Arbfloat;  // Note: the index in a dynamic array is 0-based.
      ij: Integer;
      i, j: Integer;
      p: ^ArbFloat;
      m, n: Integer;
    begin
      m := 2;
      n := 3;
      SetLength(A, m, n);
      A[0] := 1;  A[1] := 2;  A[2] := 3;  
      A[3] := 4;  A[4] := 5;  A[5] := 6;
    
      for i := 0 to m-1 do begin
        for j := 0 to n-1 do begin
          ij := i * n + j;
          Write(A[ij]);
        end;
        WriteLn;
      end;
    
      // or
      p := @A[0];   // pointer to first array element
      for i:=0 to m - 1 do begin
        for j := 0 to n - 1 do begin
          WriteLn(p^);
          inc(p);   // advance pointer to next array element
        end;
        WriteLn;
      end;
    

#### Symmetric matrices

The elements of a symmetric matrix are mirrored at the main diagonal, i.e. _a ij_ = _a ji_. In other words: a symmetric matrix is equal to its transpose: _A_ = _A_ T. 

NumLib contains some routine optimized for such matrices. They require the matrix being stored like a generic matrix in a rectangular m x n array or a linear m*n array. However, only the half of the matrix below the diagonal must be populated (but it can be, of course). 

#### Symmetric positive definite matrices

A matrix _A_ is positive definite if the product **d** T _A_ **d** is a positive number for any non-zero vector **d**. A matrix is symmetric positive definite if, additionally, _A_ T = _A_. 

This type of matrices has some interesting properties (determinant is never 0, eigenvalues are real and positive). Therefore NumLib offers specialized routines for many matrix problems with symmetric positive matrices which are superior to those for general matrices. 

The matrix is stored in the conventional way like a rectangular 2D array of _m_ rows and _n_ columns or a linear 1D array with _m_ *_n_ elements. 

#### Band matrices

A _n_ x _n_ band matrix has zero elements everywhere except for a band along the main diagonal characterized by a left and right bandwidth (_l_ and _r_ , respectively). 

The NumLib procedures for band matrices assume that the matrix stores only the elements of the band. It is expected the the matrix is passed as a 1D array. Starting at the top/left corner point of the matrix (element _A_ 11) the diagonal elements are collected by running along the rows of the matrix, from left to right and from top to bottom. In total, there will be _n_ (_l_ +1+_r_) - (_r_(_r_ +1) +  _l_(_l_ +1)) div 2 elements. The calling procedure must allocate a 1D array of at least this size for the diagonal elements. Note that a band matrix cannot be passed as a 2D array. 

As an example, suppose this 7 x 7 matrix with left bandwidth 2 and right bandwidth 1: 

[math]\displaystyle{ A = \begin{bmatrix} A_{11} & A_{12} & 0 & 0 & 0 & 0 & 0 \\\ A_{21} & A_{22} & A_{23} & 0 & 0 & 0 & 0 \\\ A_{31} & A_{32} & A_{33} & A_{34} & 0 & 0 & 0 \\\ 0 & A_{42} & A_{43} & A_{44} & A_{45} & 0 & 0 \\\ 0 & 0 & A_{53} & A_{54} & A_{55} & A_{56} & 0 \\\ 0 & 0 & 0 & A_{64} & A_{65} & A_{66} & A_{67} \\\ 0 & 0 & 0 & 0 & A_{75} & A_{76} & A_{77} \end{bmatrix} }[/math]

The 1D data array to be passed to the NumLib routines consists of these elements 
    
    
    const 
      arr: array[1..24] of ArbFloat = (    
        A11, A22,
        A21, A22, A23,
        A31, A32, A33, A34,
             A42, A43, A44, A45,
                  A53, A54, A55, A56,
                       A64, A65, A66, A67,
                            A75, A76, A77
      );
    

#### Symmetric band matrices

In a symmetric matrix, the matrix elements can be mirrored at the main diagonal of the matrix, i.e. _A_ ij = _A_ ji. All elements out side a band of width w to the left and right of the main diagonal are zero. 

Like with a generic band matrix, NumLib assumes that the only the elements of the diagonals are passed as a 1D array to the procedures dedicated to this matrix type. Since in a symmetric band matrix the left and right band widths are equal it is assumed that only the left band and the main diagonal are stored. Again the relevant diagonal elements are collected by running across the rows of the matrix from left to right and from top to bottom, stopping in each row at the main diagonal. In total, there will be _n_ (_w_ +1) -  _w_(_w_ +1) div 2 elements in the data array. 

As an example consider this 7 x 7 symmetric band matrix with band width 2: 

[math]\displaystyle{ A = \begin{bmatrix} A_{11} & A_{21} & A_{31} & 0 & 0 & 0 & 0 \\\ A_{21} & A_{22} & A_{32} & A_{42} & 0 & 0 & 0 \\\ A_{31} & A_{32} & A_{33} & A_{43} & A_{53} & 0 & 0 \\\ 0 & A_{42} & A_{43} & A_{44} & A_{54} & A_{64} & 0 \\\ 0 & 0 & A_{53} & A_{54} & A_{55} & A_{65} & A_{75} \\\ 0 & 0 & 0 & A_{64} & A_{65} & A_{66} & A_{76} \\\ 0 & 0 & 0 & 0 & A_{75} & A_{76} & A_{77} \end{bmatrix} }[/math]

The 1D array for the NumLib procedures is 
    
    
    const
      arr: array[1..18] of ArbFloat = (
        A11,
        A21, A22,
        A31, A32, A33,
             A42, A43, A44,
                  A53, A54, A55,
                       A64, A65, A66,
                            A75, A76, A77
      );
    

#### Tridiagonal matrices

A tridiagonal matrix has zero elements everywhere except for the main diagonal and the diagonal immediately above and below it. This way the matrix can be stored more efficiently than a generic matrix because all the empty elements can be dropped. The elements of an n x n square tridiagonal matrix can be stored as three vectors 

  * an array `l` with `n-1` elements for the subdiagonal
  * an array `d` with `n` elements for the main diagonal, and
  * an array `u` with `n-1` elements for the superdiagonal



    [math]\displaystyle{ A= \begin{bmatrix} d[0] & u[0] & & \dots & 0 \\\ l[1] & d[1] & u[1] & & \\\ & l[2] & d[2] & u[2] & \\\ \vdots & & \ddots & \ddots & \ddots \\\ & & l[n-1] & d[n-1] & u[n-1] \\\ 0 & & & l[n] & d[n] \end{bmatrix} }[/math]

#### Symmetric tridiagonal matrices

In these tridiagonal matrices, the subdiagonal and superdiagonal elements are equal. Consequently, NumLib requests only two, instead of three, one-dimensional arrays: 

  * an array `d` with `n` elements for the main diagonal, and
  * an array `c` with `n-1` elements for the superdiagonal and subdiagonal



    [math]\displaystyle{ A= \begin{bmatrix} d[0] & c[1] & & \dots & 0 \\\ c[1] & d[1] & c[2] & & \\\ & c[2] & d[2] & c[3] & \\\ \vdots & & \ddots & \ddots & \ddots \\\ & & c[n-1] & d[n-1] & c[n] \\\ 0 & & & c[n] & d[n] \end{bmatrix} }[/math]

  


### Complex numbers

NumLib defines the type `complex` as a Turbo Pascal `object`. Real and imaginary parts are denoted as `xreal` and `imag`, respectively. Methods for basic operations are provided: 
    
    
    type
      complex  = object
        xreal, imag : ArbFloat;
        procedure Init(r, i: ArbFloat);    // Initializes the number with real and imaginary parts r and i, resp
        procedure Add(c: complex);         // Adds another complex number 
        procedure Sub(c: complex);         // Subtracts a complex number
        function  Inp(z:complex):ArbFloat; // Calculates the inner product (a.Re * b.Re + a.Im * b.Im)
        procedure Conjugate;               // Conjugates the value (i.e. negates the imaginary part)
        procedure Scale(s: ArbFloat);      // Multiplies real and imaginary part by the real value s
        function  Norm: ArbFloat;          // Calculates the "norm", i.e. sqr(Re) + sqr(Im) 
        function  Size: ArbFloat;          // Size = sqrt(Norm) 
        function  Re: ArbFloat;            // Returns the real part of the complex number
        procedure Unary;                   // Negates real and imaginary parts
        function  Im: ArbFloat;            // Returns the imaginary part of the complex number
        function  Arg: ArbFloat;           // Calculates the phase phi of the complex number z = x + y i = r exp(i phi)
        procedure MinC(c: complex);        // Converts itself to the minimum of real and imaginary parts of itself and another value, c
        procedure MaxC(c: complex);        // Converts itself to the maximum of real and imaginary ports of itself and another value, c
        procedure TransF(var t: complex);
      end;
    

The real and imaginary parts of a complex number can also be retrieved by these stand-alone functions: 
    
    
      function Re(c: complex): ArbFloat;
      function Im(c: complex): ArbFloat;
    

  


## Basic operations with matrices and vectors (unit _omv_)

### Inner product of two vectors

The result of the inner product of two vectors **a** = [a1, a2, ... an] and **b** = [b1, b2, ... bn] (also called "dot product" or "scalar product") is a scalar, i.e. a single number. It is calculated as the sum of the element-by-element products of the vectors. Both vectors must have the same lengths. 

    [math]\displaystyle{ \mathbf{a} \cdot \mathbf{b} = \sum_{i=1}^n a_i b_i = a_1 b_1 + a_2 b_2 + \dots + a_n b_n }[/math]

NumLib provides the function `omvinp` for calculation of the inner product: 
    
    
    function omvinp(var a, b: ArbFloat; n: ArbInt): ArbFloat;
    

  * `a` and `b` are the first elements of 1-dimensional arrays representing the vectors **a** and **b** , respectively.
  * `n` defines the dimension of the vectors (count of array elements). Both vectors must have the same number of elements.



  
**Example**

Calculate the dot product of the vectors **a** = [0, 1, 2, 2, -1] and **b** = [3, -1, -2, 2, -1]. 
    
    
    program dot_product;
    
    uses
      typ, omv;
    
    const
      n = 5;
      a: array[0..n-1] of ArbFloat = (0, 1, 2, 2, -1);
      b: array[0..n-1] of Arbfloat = (3, -1, -2, 2, -1);
    
    var
      ab: ArbFloat;
      i: Integer;
    
    begin
      ab := omvinp(a[0], b[0], n);
    
      Write('Vector a = [');
      for i:= Low(a) to High(a) do
        Write(a[i]:5:0);
      WriteLn('  ]');
    
      Write('Vector b = [');
      for i:= Low(b) to High(b) do
        Write(b[i]:5:0);
      WriteLn('  ]');
    
      Write('Dot product a b = ');
      WriteLn(ab:0:0);
    end.
    

  


### Product of two matrices

The product of a m x n matrix A and a n x p matrix B is a m x p matrix C in which the elements are calculated as the inner product of the row vectors of A and the column vectors of B (see [here](<https://en.wikipedia.org/wiki/Matrix_multiplication>) for more details): 

    [math]\displaystyle{ C_{ij} = \sum_{k=0}^n A_{ik} B_{kj} }[/math]

The NumLib function to perform this multipication is `omvmmm` ("mmm" = **_m_** ultiply **_m_** atrix by **_m_** atrix): 
    
    
    procedure omvmmm(var a: ArbFloat; m, n, rwa: ArbInt;
                     var b: ArbFloat; p, rwb: ArbInt;
                     var c: ArbFloat; rwc: ArbInt);
    

  * `a` is the first element of the first input matrix _A_. The array will not be changed. 
    * `m` specifies the number of rows (or: column length) of matrix _A_.
    * `n` specifies the number or columns (or: row length) of matrix _A_.
    * `rwa` is the allocated row length of matrix _A_. This means that the array can be allocated larger than actually needed. Of course, `rwa` cannot be smaller than `n`.


  * `b` is the first element of the second input matrix _B_. Again, this array will not be changed. 
    * `p` specifies the number or columns (or: row length) of matrix _B_. The number of rows is already defined by the number of columns of matrix _A_ , `n`.
    * `rwb` is the allocated row length of matrix _B_. `rwb` cannot be smaller than `p`.


  * `c` is the first element of the output matrix _C_. After the calculation, this and the following array elements will contain the elements of the product matrix. 
    * The size of the output matrix is `m` rows x `p` columns as set up by the input matrices _A_ and _B_.
    * `rwc` is the allocated row length of matrix _C_. `rwc` cannot be smaller than `p`.



  
**Example**

Calculate the product of the matrices 

    [math]\displaystyle{ A = \begin{bmatrix} 3 & \ 5 & 4 & 1 & -4 \\\ -2 & \ 3 & 4 & 1 & 0 \\\ 0 & \ 1 & -1 & -2 & 5 \end{bmatrix} \ \ \ \ \ B = \begin{bmatrix} 3 & 4 & -2 & 0 \\\ 0 & -2 & 2 & 1 \\\ -1 & 0 & 3 & 4 \\\ -2 & -3 & 0 & 1 \\\ 1 & 3 & 0 & -1 \end{bmatrix} }[/math]
    
    
    program matrix_multiplication;
    
    uses
      typ, omv;
    
    const
      m = 3;
      n = 5;
      p = 4;
    
      A: array[1..m, 1..n] of ArbFloat = (
        ( 3,  5,  4,  1, -4),
        (-2,  3,  4,  1,  0),
        ( 0,  1, -1, -2,  5)
      );
      B: array[1..n, 1..p] of ArbFloat = (
        ( 3,  4, -2,  0),
        ( 0, -2,  2,  1),
        (-1,  0,  3,  4),
        (-2, -3,  0,  1),
        ( 1,  3,  0, -1)
      );
    
    var
      C: array[1..m, 1..p] of ArbFloat;
      i, j: Integer;
    
    begin
      // Calculate product
      omvmmm(
        A[1,1], m, n, n,
        B[1,1], p, p,
        C[1,1], p
      );
    
      // Print A
      WriteLn('A = ');
      for i:= 1 to m do begin
        for j := 1 to n do
          Write(A[i, j]:10:0);
        WriteLn;
      end;
      WriteLn;
    
      // Print B
      WriteLn('B = ');
      for i:= 1 to n do begin
        for j := 1 to p do
          Write(B[i, j]:10:0);
        WriteLn;
      end;
      WriteLn;
    
      // Print C
      WriteLn('C = A B = ');
      for i:= 1 to m do begin
        for j := 1 to p do
          Write(C[i, j]:10:0);
        WriteLn;
      end;
    end.
    

  


### Product of a matrix with a vector

A matrix _A_ of size m x n multiplied with a vector **b** of length n yields another vector of length m. The elements of the result vector **c** are the dot products of the matrix rows with **b**. Note that the length of vector **b** must be equal to the number of columns of matrix _A_. 

    [math]\displaystyle{ \begin{align*} \mathbf{c} = A\ \mathbf{b}= \left[ \begin{array}{cccc} a_{11} & a_{12} & a_{13} & \ldots & a_{1n}\\\ a_{21} & a_{22} & a_{23} & \ldots & a_{2n}\\\ \vdots & \vdots & \vdots & \ddots & \vdots\\\ a_{m1} & a_{m2} & a_{m3} & \ldots & a_{mn} \end{array} \right] \ \ \left[ \begin{array}{c} b_1\\\ b_2\\\ b_3\\\ \vdots\\\ b_n \end{array} \right] = \left[ \begin{array}{c} a_{11}b_1+a_{12}b_2 + a_{13}b_3 + \cdots + a_{1n} b_n\\\ a_{21}b_1+a_{22}b_2 + a_{23}b_3 + \cdots + a_{2n} b_n\\\ \vdots\\\ a_{m1}b_1+a_{m2}b_2 + a_{m3}b_3 + \cdots + a_{mn} b_n\\\ \end{array} \right]. \end{align*} }[/math]

NumLib's procedure for this task is called `omvmmv` ("mmv" = **_m_** ultiply a **_m_** atrix with a **_v_** ector): 
    
    
    procedure omvmmv(var a: ArbFloat; m, n, rwidth: ArbInt; var b, c: ArbFloat);
    

  
**Example**

Calculate the product 

    [math]\displaystyle{ \begin{align*} \mathbf{c} = A\ \mathbf{b}= \left[ \begin{array}{cccc} 3 & 5 & 4 & 1 & -4\\\ -2 & 3 & 4 & 1 & 0\\\ 0 & 1 & -1 & -2 & 5 \end{array} \right] \ \ \left[ \begin{array}{c} 3\\\ 0\\\ -1\\\ -2\\\ 1 \end{array} \right] \end{align*} }[/math]
    
    
    program matrix_vector_multiplication;
    
    uses
      typ, omv;
    
    const
      m = 3;
      n = 5;
      p = 4;
    
      A: array[1..m, 1..n] of ArbFloat = (
        ( 3,  5,  4,  1, -4),
        (-2,  3,  4,  1,  0),
        ( 0,  1, -1, -2,  5)
      );
      b: array[1..n] of ArbFloat = (
        3, 0, -1, -2, 1
      );
    
    var
      c: array[1..m] of ArbFloat;
      i, j: Integer;
    
    begin
      // Calculate product c = A b
      omvmmv(
        A[1,1], m, n, n,
        b[1],
        c[1]
      );
    
      // Print A
      WriteLn('A = ');
      for i:= 1 to m do begin
        for j := 1 to n do
          Write(A[i, j]:10:0);
        WriteLn;
      end;
      WriteLn;
    
      // Print vector b
      WriteLn('b = ');
      for i:= 1 to n do
        Write(b[i]:10:0);
      WriteLn;
      WriteLn;
    
      // Print result vector c
      WriteLn('c = A b = ');
      for i:= 1 to m do
        Write(c[i]:10:0);
      WriteLn;
    end.
    

  


### Transpose matrix

The [transpose matrix](<https://en.wikipedia.org/wiki/Transpose>) _A T_ of matrix _A_ is obtained by flipping rows and columns: 

    [math]\displaystyle{ \begin{align*} {A} = \left[ \begin{array}{cccc} a_{11} & a_{12} & a_{13} & \ldots & a_{1n}\\\ a_{21} & a_{22} & a_{23} & \ldots & a_{2n}\\\ \vdots & \vdots & \vdots & \ddots & \vdots\\\ a_{m1} & a_{m2} & a_{m3} & \ldots & a_{mn} \end{array} \right] \ \ \ {A^T} = \left[ \begin{array}{cccc} a_{11} & a_{21} & \ldots & a_{m1}\\\ a_{12} & a_{22} & \ldots & a_{m2}\\\ a_{13} & a_{23} & \ldots & a_{m3} \\\ \vdots & \vdots & \ddots & \vdots \\\ a_{1n} & a_{2n} & \ldots & a{mn} \end{array} \right] \end{align*} }[/math]

Use the procedure `omvtrm` to perform this operation with NumLib: 
    
    
    procedure omvtrm(
      var a: ArbFloat; m, n, rwa: ArbInt; 
      var c: ArbFloat; rwc: ArbInt
    );
    

  * `a` denotes the first element of the input matrix _A_. The elements of this array are not changed by the procedure. 
    * `m` is the number of rows of matrix _A_.
    * `n` is the number of columns of matrix _A_.
    * `rwa` is the number of allocated columns of _A_. This way the array of _A_ can be larger than acutally needed.
  * `c` is the first element of the transposed matrix _A T_. It has `n` rows and `m` columns. 
    * `rwc` is the number of allocated columns for the transposed matrix.



  
**Example**

Obtain the transpose of the 2x4 matrix 

    [math]\displaystyle{ {A} = \left[ \begin{array}{cccc} 1 & 2 & 3 & 4 \\\ 11 & 22 & 33 & 44 \end{array} \right] }[/math]
    
    
    program transpose;
    
    uses
      typ, omv;
    
    const
      m = 2;
      n = 4;
    
      A: array[1..m, 1..n] of ArbFloat = (
        ( 1,  2,  3,  4),
        (11, 22, 33, 44)
      );
    
    var
      C: array[1..n, 1..m] of ArbFloat;
      i, j: Integer;
    
    begin
      // Transpose A
      omvtrm(
        A[1,1], m, n, n,
        C[1,1], m
      );
    
      // Print A
      WriteLn('A = ');
      for i:= 1 to m do begin
        for j := 1 to n do
          Write(A[i, j]:10:0);
        WriteLn;
      end;
      WriteLn;
    
      // Print C
      WriteLn('AT = ');
      for i:= 1 to n do begin
        for j := 1 to m do
          Write(C[i, j]:10:0);
        WriteLn;
      end;
    end.
    

  


### Norm of vectors and matrices

A norm is a function which assigns a positive "length" to a [vector **a**](<https://en.wikipedia.org/wiki/Norm_\(mathematics\)>) or a [matrix _M_](<https://en.wikipedia.org/wiki/Matrix_norm>). 

In case of vectors, NumLib supports these norms: 

  * The **1-norm** of a vector is defined as the sum of the absolute values of the vector components. It is also called "Taxicab" or "Manhattan" norm because it corresponds to the distance that a taxis has to drive in a rectangular grid of streets.



    [math]\displaystyle{ \|a\|_1 = \sum_{i=1}^n |{a_i}| }[/math]

  * The **2-norm** (also: Euclidian norm) corresponds to the distance of point **a** from the origin.



    [math]\displaystyle{ \|a\|_2 = \sqrt{\sum_{i=1}^n {a_i}^2} }[/math]

  * The **maximum infinite norm** of a vector is the maximum absolute value of its components.



    [math]\displaystyle{ \|a\|_\infty = \max({a_1}, {a_2}, ... {a_n}) }[/math]

The norms of matrices, as implemented by NumLib, are usually defined by row or column vectors: 

  * The **1-norm** of a matrix is the maximum of the absolute column sums:



    [math]\displaystyle{ \|M\|_1 = \max_{1 \le j \le {n}} \sum_{i=1}^m|M_{ij}| }[/math]

  * The **maximum infinite norm** of a matrix is the maximum of the absolute row sum.



    [math]\displaystyle{ \|M\|_\infty = \max_{1 \le i \le\ m} \sum_{j=1}^n |M_{ij}| }[/math]

  * Only the **Frobenius norm** is calculated by individual matrix elements. It corresponds to the 2-norm of vectors, only the sum runs over all matrix elements:



    [math]\displaystyle{ \|M\|_F = \sqrt{\sum_{i=1}^m \sum_{j=1}^n {M_{ij}}^2} }[/math]

These are the NumLib procedure for calculation of norms: 
    
    
    function omvn1v(var a: ArbFloat; n: ArbInt): ArbFloat;               // 1-norm of a vector
    function omvn1m(var a: ArbFloat; m, n, rwidth: ArbInt): ArbFloat;    // 1-norm of a matrix
    function omvn2v(var a: ArbFloat; n: ArbInt): ArbFloat;               // 2-norm of a vector
    function omvnfm(Var a: ArbFloat; m, n, rwidth: ArbInt): ArbFloat;    // Frobenius norm of a matrix
    function omvnmv(var a: ArbFloat; n: ArbInt): ArbFloat;               // Maximum infinite norm of a vector
    function omvnmm(var a: ArbFloat; m, n, rwidth: ArbInt): ArbFloat;    // Maximum infinite norm of a matrix
    

  * `a` is the first element of the vector or matrix of which the norm is to be calculated
  * `m`, in case of a matrix norm, is the number of matrix rows.
  * `n` is the number of vector elements in case of a vector norm, or the number of columns in case of a matrix norm.
  * `rwidth`, in case of a matrix norm, is the allocated count of columns. This way a larger matrix can be allocated than actually needed.



**Example**
    
    
    program norm;
    
    uses
      typ, omv;
    
    const
      n = 2;
      m = 4;
      a: array[0..m-1] of ArbFloat = (0, 1, 2, -3);
      b: array[0..m-1, 0..n-1] of Arbfloat = ((3, -1), (-2, 2), (0, -1), (2, 1));
    
    var
      i, j: Integer;
    
    begin
      Write('Vector a = [');
      for i:= Low(a) to High(a) do
        Write(a[i]:5:0);
      WriteLn('  ]');
      WriteLn('        1-norm of vector a: ', omvn1v(a[0], m):0:3);
      WriteLn('        2-norm of vector a: ', omvn2v(a[0], m):0:3);
      WriteLn('  maximum norm of vector a: ', omvnmv(a[0], m):0:3);
      WriteLn;
    
      WriteLn('Matrix b = ');
      for i:= 0 to m-1 do begin
        for j := 0 to n-1 do
          Write(b[i,j]:5:0);
        WriteLn;
      end;
      WriteLn('        1-norm of matrix b: ', omvn1m(b[0, 0], m, n, n):0:3);
      WriteLn('Forbenius norm of matrix b: ', omvnfm(b[0, 0], m, n, n):0:3);
      WriteLn('  maximum norm of matrix b: ', omvnmm(b[0, 0], m, n, n):0:3);
    
      ReadLn;
    end.
    

  


## Determinant of a matrix (unit _det_)

Unit _det_ provides a series of routines for calculation of the [determinant](<https://en.wikipedia.org/wiki/Determinant>) of a square matrix. Besides a procedure for general matrices there are also routines optimized for special matrix types (tridiagonal, symmetric). 

In order to avoid possible overflows and underflows a factor may be split off during calculation, and the determinant is returned as a floating point number _f_ times a multiplier _8 k_. In many cases, however, _k = 0_ , and the determinant simply is equal to _f_. 

  


### Determinant of a standard matrix

The term "standard matrix" means here a matrix with is stored in the standard NumLib storage as described here, i.e. the matrix is stored as a 2D or 1D array of real numbers. The 2D array has at least _n_ rows and _n_ columns, the 1D array is dimensioned to contain at least _n_ 2 elements. 

Without making any assumptions on the properties of the matrix the determinant can be calculated by the procedure `detgen`. More efficient algorithms can be used, however, if the matrix has these particular properties: 

  * symmetric matrix --> use procedure `detgsy`.
  * symmetric positive definite matrix --> use procedure `getgpd`.



  

    
    
    procedure detgen(n, rwidth: ArbInt; var a, f: ArbFloat; var k, term: ArbInt);    // --> generic matrix
    procedure detgsy(n, rwidth: ArbInt; var a, f: ArbFloat; var k, term: ArbInt);    // --> symmetric matrix
    procedure detgpd(n, rwidth: ArbInt; var a, f: ArbFloat; var k, term: ArbInt);    // --> symmetric positive definite matrix
    

  * `n` determines the number of rows and columns of the input matrix (it must be square matrix).
  * `rwidth` specifies the allocated row length of the input matrix. This takes care of the fact that matrices can be allocated larger than actually needed.
  * `a` is the first (i.e. top/left) element of the input matrix. The entire matrix must be contained within the same allocated memory block.
  * `f` and `k` return the determinant of the matrix. The determinant is given by _f * 8 k_. Usually _k = 0_ , and the determinant simply is equal to _f_.



  
**Example**

Calculate the determinant of the 4x4 matrix 

    [math]\displaystyle{ A= \left[ \begin{array}{rrrr} 4 & 2 & 4 & 1 \\\ 30 & 20 & 45 & 12 \\\ 20 & 15 & 36 & 10 \\\ 35 & 28 & 70 & 20 \end{array} \right] }[/math]

The matrix is positive definite because all elements are positive and the product **b** T _A_ **b** cannot become zero or negative for any non-zero vector **b**. While the routine `detgpd` is the best for this matrix type the code below contains also commented-out calls to the other routines. 
    
    
    program determinant_spd_matrix;   // "spd" = symmetric positive definite
    
    uses
      math, typ, det;
    
    const
      n = 4;
    
    const
      a: array[1..n, 1..n] of ArbFloat = (
        ( 4,  2,  4,  1)
        (30, 20, 45, 12)
        (20, 15, 36, 10)
        (35, 28, 70, 20)
      );
    
    var
      f: ArbFloat;
      k: Integer;
      term: Integer = 0;
      i, j: Integer;
    
    begin
      // write input matrix
      WriteLn('A = ');
      for i := 1 to n do begin
        for j := 1 to n do
          Write(a[i, j]:10:0);
        WriteLn;
      end;
      WriteLn;
    
      // Calculate determinant of symmetric positive definite matrix
      detgpd(n, n, a[1, 1], f, k, term);
    
      // or uncomment to use one of the other methods
      // detgen(n, n, a[1, 1], f, k, term);
      // detgsy(n, n, a[1, 1], f, k, term);
    
      if term = 1 then
        WriteLn('Determinant of A = ', f * intpower(8, k):0:9, ' (k = ', k, ')')
      else
        WriteLn('Incorrect input');
    end.
    

### Determinant of a band matrix

If a matrix has the shape of a band matrix then its determinant is best calculated by using the procedure `detgba`
    
    
    procedure detgba(n, l, r: ArbInt; var a, f: ArbFloat; var k, term:ArbInt);
    

  * `n` denotes the number of columns and rows of the matrix (it must be square).
  * `l` identifies the left bandwidth, i.e. how many diagonals the band extends to the left, or below, the main diagonal.
  * `r` identifies the right bandwidth, i.e. how many diagons the band extends to the right, or above, the main diagonsl.
  * `a` specifies the first element of a 1D array which contains the elements of the diagonal band, see this. This array contains only the band elements and is obtained by running across the rows of the band matrix from left to right and top to bottom, starting at element _A_ 11. It must be dimensioned to contain at least `n*(l+1+r) - (r*(r+1)+l*(l+1)) div 2` elements.
  * `f` and `k` return the determinant of the matrix. The determinant is given by _f_ * 8 _k_. Usually _k = 0_ , and the determinant simply is equal to _f_.



  
**Example**

Calculate the determinant of the 5x5 band matrix with left bandwidth _l_ = 2 and right bandwidth _r_ = 1 

    [math]\displaystyle{ A = \begin{bmatrix} 1 & 2 & 0 & 0 & 0 \\\ 2 & 1 & 1 & 0 & 0 \\\ 1 & 3 & 0 & 1 & 0 \\\ 0 & 1 & 1 & 1 & 1 \\\ 0 & 0 & 1 & 1 & 1 \end{bmatrix} }[/math]
    
    
    program determinant_band_matrix;
    
    uses
      math, typ, det;
    
    const
      n = 5;
      L = 2;
      R = 1;
      nelements = n*(L+1+R) - (R*(R+1) + L*(L+1)) div 2;
    
    const
      // Diagonal elements arranged in rows, elements outside of the band omitted!
      a: array[1..nelements] of Arbfloat = (
         1, 2,
         2, 1, 1,
         1, 3, 0, 1,
         1, 1, 1, 1,
         1, 1, 1
        );
    var
      f: ArbFloat;
      term: Integer = 0;
      i, i1, i2, j, k: Integer;
    
    begin
      // Write diagonal elements
      WriteLn('n = ', n);
      Writeln('Left band width L = ', L);
      WriteLn('Right band width R = ', R);
      Write('Diagonal elements of A = ', a[1]:0:0);
      for k := 2 to nelements do
        Write(a[k]:5:0);
      WriteLn;
      WriteLn;
    
      // write reconstructed band input matrix (not needed for calculation)
      WriteLn('Reconstructed A = ');
      k := 1;
      for j := 1 to n do begin
        i1 := j - L;
        i2 := i1 + L + R;
        for i:=1 to i1-1 do
          Write(0.0:5:0);
        for i := i1 to i2 do
          if (i >= 1) and (i <= n) then begin
            Write(a[k]:5:0);
            inc(k);
          end;
        for i:= i2 + 1 to n do
          if i <= n then
            Write(0.0:5:0);
        WriteLn;
      end;
      WriteLn;
    
      // Calculate determinant
      detgba(n, l, r, a[1], f, k, term);
      if term = 1 then
        WriteLn('Determinant of A = ', f * intpower(8, k):0:9, ' (k = ', k, ')')
      else
        WriteLn('Incorrect input');
    end.
    

  


### Determinant of a symmetric positive definite band matrix

If a matrix has the shape of a band matrix and is symmetric positive definite then `detgpb` is the dedicated routine for calculation of its determinant. 
    
    
    procedure detgpb(n, w: ArbInt; var a, f: ArbFloat; var k, term:ArbInt);
    

  * `n` denotes the number of columns and rows of the matrix (it must be square).
  * `w` identifies the bandwidth, i.e. how many diagonals the band extends to the left or to the right of the main diagonal. Left and right bandwidth must be equal.
  * `a` specifies the first element of a 1D array which contains the elements of the diagonal band, see this. This array contains only the left band elements and the elements of the main diagonal. It is obtained by running across the rows of the band matrix from left to right and top to bottom, but stopping at the main diagonal - the right band is ignored since it is equal to the left band. The array must be dimensioned to contain at least `n*(w+1) - (w*(w+1)) div 2` elements.
  * `f` and `k` return the determinant of the matrix. The determinant is given by _f_ * 8 _k_. Usually _k = 0_ , and the determinant simply is equal to _f_.



  
**Example**

Calculate the determinant of the 7x7 symmetric band matrix with bandwidth _w_ = 2 [math]\displaystyle{ A= \begin{bmatrix} 5 & -4 & 1 & & & & \\\ -4 & 6 & -4 & 1 & & & \\\ 1 & -4 & 6 & -4 & 1 & & \\\ & 1 & -4 & 6 & -4 & 1 & \\\ & & 1 & -4 & 6 & -4 & 1 \\\ & & & 1 & -4 & 6 & -4 \\\ & & & & 1 & -4 & 5 \end{bmatrix} }[/math]
    
    
    program determinant_spd_band_matrix;     // "spd" = symmetric positive definite
    
    uses
      math, typ, det;
    
    const
      n = 7;
      w = 2;
      nelements = n * (w + 1) - (w * (w + 1)) div 2;
    
    const
      // Band elements arragned in rows, elements outside of the band omitted!
      a: array[1..nelements] of Arbfloat = (
                 5,
             -4, 6,
          1, -4, 6,
          1, -4, 6,
          1, -4, 6,
          1, -4, 6,
          1, -4, 5
        );
    
      function MatrixIndexToArrayIndex(i, j, n, w: Integer): Integer;
    
        function DiagElementIndex(i: Integer): Integer;
        var
          k: Integer;
        begin
          Result := 1;
          if i = 1 then
            exit;
          // Rows truncated at left
          for k := 2 to w do begin
            Result := Result + k;
            if k = i then
              exit;
          end;
          // full rows and those truncated at right
          for k := w+1 to n do begin
            Result := Result + w + 1;
            if k = i then
              exit;
          end;
          Result := n;
        end;
    
      var
        d: Integer;
      begin
        if j > i then begin
          Result := MatrixIndexToArrayIndex(j, i, n, w);
          exit;
        end;
    
        Result := DiagElementIndex(i);
        if (i = j) then
          exit;
    
        d := i - j;
        if d > w then
          Result := -1
        else begin
          dec(Result, d);
          if (Result < 1) then
            Result := -1
        end;
      end;
    
    var
      i, j, k: Integer;
      f: ArbFloat;
      term: Integer = 0;
    begin
      // Write diagonal elements
      WriteLn('n = ', n);
      Writeln('(One-sided) band width W = ', w);
      Write('Diagonal elements of A = ', a[1]:0:0);
      for k := 2 to nelements do
        Write(a[k]:3:0);
      WriteLn;
      WriteLn;
    
      // write reconstructed band input matrix (not needed for calculation)
      WriteLn('Reconstructed A = ');
      k := 1;
      for i := 1 to n do begin
        for j := 1 to n do begin
          k := MatrixIndexToArrayIndex(i, j, n, w);
          if k = -1 then
            Write(0.0:3:0)
          else
            Write(a[k]:3:0);
        end;
        WriteLn;
      end;
      WriteLn;
    
      // Calculate determinant
      detgpb(n, w, a[1], f, k, term);
      if term = 1 then
        WriteLn('Determinant of A = ', f * intpower(8, k):0:9, ' (k = ', k, ')')
      else
        WriteLn('Incorrect input');
    end.
    

  


### Determinant of a tridiagonal matrix

Calculation of the determinant of a tridiagonal matrix is done by the procedure `detgtr`: 
    
    
    procedure detgtr(n: ArbInt; var l, d, u, f: ArbFloat; var k, term:ArbInt);
    

  * `n` denotes the number of columns and rows of the matrix (it must be square).
  * `l` specifies the first element in the subdiagonal of the matrix. This 1D array must be dimensioned to at least `n-1` elements.
  * `d` specifies the first element along the main diagonal of the matrix. This 1D array must be dimensioned to contain at least `n` elements.
  * `u` specifies the first element along the superdiagonal of the matrix. The array must be dimensioned to have at least `n-1` elements.
  * `f` and `k` return the determinant of the matrix. The determinant is given by _f * 8 k_. Usually _k = 0_ , and the determinant simply is equal to _f_.



  
**Example** Calculate the determinant of the matrix 

    [math]\displaystyle{ A= \left( \begin{array}{rrrr} 1 & 1 & & \\\ 2 & 1 & 1 & \\\ & 1 & 1 & 0 \\\ & & 1 & 2 \end{array} \right) }[/math]
    
    
    program determinant_tridiagonal_matrix;
    
    {$mode objfpc}{$H+}
    
    uses
      math, typ, det;
    
    const
      n = 4;
    
    const
      u: array[0..n-2] of ArbFloat = (-1, -1, -1   );       // superdiagonal
      d: array[0..n-1] of ArbFloat = ( 2,  2,  2,  2);      // main diagonal
      l: array[1..n-1] of ArbFloat = (    -1, -1, -1);      // subdiagonal
    
    var
      f: ArbFloat;
      k: Integer;
      term: Integer = 0;
      i, j: Integer;
    
    begin
      // write input matrix
      WriteLn('A = ');
      for i := 0 to n-1 do begin
        for j := 0 to i-2 do
          Write(0.0:5:0);
        if i > 0 then
          Write(l[i]:5:0);
        Write(d[i]:5:0);
        if i < n-1 then
          Write(u[i]:5:0);
        for j:=i+2 to n-1 do
          Write(0.0:5:0);
        WriteLn;
      end;
      WriteLn;
    
      // Calculate determinant
      detgtr(n, l[1], d[0], u[0], f, k, term);
      if term = 1 then
        WriteLn('Determinant of A = ', f * intpower(8, k):0:9, ' (k = ', k, ')')
      else
        WriteLn('Incorrect input');
    
      ReadLn;
    
    end.
    

  


## Inverse of a matrix (unit _inv_)

Suppose a square matrix _A_. Then the matrix _A_ -1 is the inverse of _A_ if the product _A_ -1 _A_ is the identity matrix _I_ (all elements zero, except for the diagonal elements which are 1). 

The inverse matrix exists if the determinant of _A_ is not zero. 

    [math]\displaystyle{ A^{-1} A = I = \begin{bmatrix} 1 & & 0 \\\ & \ddots & \\\ 0 & & 1 \end{bmatrix} }[/math]

NumLib provides various routines for calculation of the inverse matrix. Which algorithm is used depends on the properties of the matrix _A_. All routines discussed here have in common that the matrix is stored in the standard NumLib way, i.e. as a 2D array of real numbers with _n_ rows and _m_ columns, or as a 1D array with _n m_ elements. 

No special requirements are made for the matrix passed to `invgen`, except that it is square. The calculation in this routine is based on [LU decomposition with partial pivoting](<https://en.wikipedia.org/wiki/LU_decomposition>). If the matrix is symmetric, it can be processed by `invgsy` which uses a reduction to tridiagonal form internally. A symmetric positive definite matrix can be passed to `invgpd` where the inverse is calculated by Cholesky decomposition. 
    
    
    procedure invgen(n, rwidth: ArbInt; var ai: ArbFloat; var term: ArbInt);   // --> generic matrix
    procedure invgsy(n, rwidth: ArbInt; var ai: ArbFloat; var term: ArbInt);   // --> symmetric matrix
    procedure invgpd(n, rwidth: ArbInt; var ai: ArbFloat; var term: ArbInt);   // --> symmetric positive definite matrix
    

  * `n` indicates the size of the matrix.
  * `rwidth` is length of the rows of the two-dimensional array which holds the matrix. Normally `n = rwidth`, but the matrix could have been declared larger than actually needed; in this case `rwidth` refers to the declared size of the data array.
  * `ai` is the first (top-left) element of the matrix in the data array. After the calculation the input data are **overwritten by the elements of the inverse matrix** in case of successful completion (`term = 1`); in other cases, the matrix elements are undefined.
  * `term` provides an error code: 
    * 1 - successful completion, the elements of the inverse matrix can be found at the same place as the input matrix.
    * 2 - the inverse could not be calculated because the input matrix is (almost) singular.
    * 3 - incorrect input data, `n < 1`.



  
**Example**

Calculate the inverse of the symmetric matrix [math]\displaystyle{ A = \left[ \begin{array}{rrrr} 5 & 7 & 6 & 5 \\\ 7 & 10 & 8 & 7 \\\ 6 & 8 & 10 & 9 \\\ 5 & 7 & 9 & 10 \end{array} \right] }[/math] This matrix is symmetric and positive definite (the product **b** T _A_ **b** with any vector **b** = [b1, b2, b3]T cannot become zero or negative since all elements of this matrix are positive). Therefore, `invgpd` is best-suited for this task although the other routines can be used as well (uncomment their calls below). 
    
    
    program inverse_matrix;  
    
    uses
      typ, inv, omv;
    
    const
      n = 4;
      D = 5;
    
    var
      // a is the input matrix to be inverted
      // Note that this is matrix must be symmetric positive definite.
      a: array[1..n, 1..n] of ArbFloat = (
        (5,  7,  6,  5),
        (7, 10,  8,  7),
        (6,  8, 10,  9),
        (5,  7,  9, 10)
      );
      b: array[1..n, 1..n] of Arbfloat;
      c: array[1..n, 1..n] of ArbFloat;
      term: Integer = 0;
      i, j: Integer;
    
    begin
      // write input matrix
      WriteLn('a = ');
      for i := 1 to n do begin
        for j := 1 to n do
          Write(a[i, j]:10:D);
        WriteLn;
      end;
      WriteLn;
    
      // Store input matrix because it will be overwritten by the inverse of a
      for i := 1 to n do
        for j := 1 to n do
          b[i, j] := a[i, j];
    
      // Calculate inverse  -- uncomment the procedure to be used.
      //invgen(n, n, a[1, 1], term);
      //invgsy(n, n, a[1, 1], term);
      invgpd(n, n, a[1, 1], term);
    
      // write inverse
      WriteLn('a^(-1) = ');
      for i := 1 to n do begin
        for j := 1 to n do
          Write(a[i, j]:10:D);
        WriteLn;
      end;
      WriteLn;
    
      // Check validity of result by multiplying inverse with saved input matrix
      omvmmm(a[1, 1], n, n, n,
             b[1, 1], n, n,
             c[1, 1], n);
    
      // write inverse
      WriteLn('a^(-1) x a = ');
      for i := 1 to n do begin
        for j := 1 to n do
          Write(c[i, j]:10:D);
        WriteLn;
      end;
    end.
    

  


## Solving systems of linear equations (unit _sle_)

A [system of linear equations](<https://en.wikipedia.org/wiki/System_of_linear_equations>) (or linear system) is a collection of two or more linear equations involving the same set of variables, _x 1 ... xn_. 

    [math]\displaystyle{ \begin{cases} a_{11} x_1 + a_{12} x_2 + \dots + a_{1n} x_n = b_1 \\\ a_{21} x_1 + a_{22} x_2 + \dots + a_{2n} x_n = b_n \\\ \ \ \ \vdots \\\ a_{m1} x_1 + a_{m2} x_2 + \dots + a_{mn} x_n = b_m \end{cases} }[/math]

In order to find a solution which satisfies all equations simultaneously the system of equations is usually converted to a matrix equation _A_ **x** = **b** , where 

    [math]\displaystyle{ A = \left [ \begin{array}{ccc} a_{11} & a_{12} & \dots & a_{1n} \\\ a_{21} & a_{22} & \dots & a_{2n} \\\ \vdots & \vdots & \ddots & \vdots \\\ a_{m1} & a_{m2} & \dots & a_{mn} \end{array} \right ], \ \mathbf{x} = \left [ \begin{array}{c} x_1 \\\ x_2 \\\ \vdots \\\ x_n \end{array} \right ], \ \mathbf{b} = \left [ \begin{array}{c} b_1 \\\ b_2 \\\ \vdots \\\ b_m \end{array} \right ] }[/math]

NumLib offers several procedures to solve the matrix equation depending on the properties of the matrix. 

  


### Square matrices with standard storage

Here we describe several routines in which the matrix of the equation system is square, i.e. there are as many equations as unknowns. This matrix is assumed to be passed to the procedures in the standard NumLib way as the first element of a 2D or 1D array. The 2D array must be dimensioned to contain at least _n_ rows and _n_ columns, the 1D array must contain at least _n_ 2 elements. 

The most general procedure is `slegen` and applied to any square matrix _A_. The system is solved using the Gauss elimination procedure with partial pivoting. 

If the matrix _A_ is symmetric then the reduction to tridiagonal form generally is more stable. This is done in routine `slegsy`. 

If _A_ , furthermore, is a symmetric positive definite matrix, then the Cholesky method is best-suited. This is applied in routine `slegpd`. 
    
    
    procedure slegen(n, rwidth: ArbInt; var a, b, x, ca: ArbFloat; var term: ArbInt);   // generic matrix
    procedure slegsy(n, rwidth: ArbInt; var a, b, x, ca: ArbFloat; var term: ArbInt);   // symmetric matrix
    procedure slegpd(n, rwidth: ArbInt; var a, b, x, ca: ArbFloat; var term: ArbInt);   // symmetric positive definite matrix
    

  * `n` is the number of unknown variables. It must be the same as the number of columns and rows of the coefficent matrix.
  * `rwidth` identifies the allocated number of columns of the coefficient matrix. This is useful if the matrix is allocated larger than needed. It is required that `n <= rwidth`.
  * `a` is the first (i.e. top left) element of the coefficient matrix _A_ ; the other elements must follow within the same allocated memory block. The matrix will not be changed during the calculation.
  * `b` is the first element of the array containing the constant vector **b**. The array length at least must be equal to `n`. The vector will not be changed during the calculation.
  * `x` is the first element of the array to receive the solution vector **x**. It must be allocated to contain at least `n` values.
  * `ca` is a parameter to describe the accuracy of the solution.
  * `term` returns an error code: 
    * 1 - successful completion, the solution vector **x** is valid
    * 2 - the solution could not have been determined because the matrix is (almost) singular.
    * 3 - error in input values: `n < 1`.



  
**Example**

Solve this system of linear equations: 

    [math]\displaystyle{ \begin{cases} 5 x_1 + 7 x_2 + 6 x_3 + 5 x_4 = 57 \\\ 7 x_1 + 10 x_2 + 8 x_3 + 7 x_4 = 79 \\\ 6 x_1 + 8 x_2 + 10 x_3 + 9 x_4 = 88 \\\ 5 x_1 + 7 x_2 + 9 x_3 + 10 x_4 = 86 \end{cases} \qquad \Rightarrow \qquad A= \left[ \begin{array}{rrrr} 5 & 7 & 6 & 5 \\\ 7 & 10 & 8 & 7 \\\ 6 & 8 & 10 & 9 \\\ 5 & 7 & 9 & 10 \end{array} \right] , \; \mathbf{b} = \left[ \begin{array}{r} 57 \\\ 79 \\\ 88 \\\ 86 \end{array} \right] }[/math]

This matrix is symmetric positive definite. Therefore, all routines presented here can be applied. Uncomment the requested line in the code below. As a test the result vector **x** is multiplied to matrix _A_ , and the result must equal the constant vector **b**. 
    
    
    program solve_sys_lin_eq;
    
    uses
      typ, sle, omv;
    
    const
      n = 4;
      A: array[1..n, 1..n] of ArbFloat = (
        ( 5,  7,  6,  5),
        ( 7, 10,  8,  7),
        ( 6,  8, 10,  9),
        ( 5,  7,  9, 10)
      );
      b: array[1..n] of ArbFloat = (
        57, 79, 88, 86
      );
    
    var
      x: array[1..n] of ArbFloat;
      b_test: array[1..n] of ArbFloat;
      ca: ArbFloat;
      i, j: Integer;
      term: Integer;
    
    begin
      WriteLn('Solve matrix system A x = b');
      WriteLn;
    
      // Print A
      WriteLn('Matrix A = ');
      for i:= 1 to n do begin
        for j := 1 to n do
          Write(A[i, j]:10:0);
        WriteLn;
      end;
      WriteLn;
    
      // Print b
      WriteLn('Vector b = ');
      for i:= 1 to n do
        Write(b[i]:10:0);
      WriteLn;
      WriteLn;
    
      // Solve - select one of these methods
      slegen(n, n, A[1, 1], b[1], x[1], ca, term);
    //  slegsy(n, n, A[1, 1], b[1], x[1], ca, term);
    //  slegpd(n, n, A[1, 1], b[1], x[1], ca, term);
    
      if term = 1 then begin
        WriteLn('Solution vector x = ');
        for i:= 1 to n do
          Write(x[i]:10:0);
        WriteLn;
        WriteLn;
    
        omvmmv(A[1,1], n, n, n, x[1], b_test[1]);
    
        WriteLn('Check result: A x = (must be equal to b)');
        for i:= 1 to n do
          Write(b_test[i]:10:0);
        WriteLn;
      end
      else
        WriteLn('Error');
    
      ReadLn;
    
    end.
    

  


### Band matrix

A special solution, `slegba`, is implemented for a band matrix, i.e. a matrix in which all elements are zero outside a band of width _l_ below and of width _r_ above the main diagonal. 
    
    
    procedure slegba(n, l, r: ArbInt; var a, b, x, ca: ArbFloat; var term:ArbInt);
    

  * `n` denotes the number of columns and rows of the matrix (it must be square).
  * `l` identifies the left bandwidth, i.e. the number of diagonals the band extends below (or to the left of) the main diagonal.
  * `r` identifies the right bandwidth, i.e. the number of diagonals the band extends abofe (or to the right of) the main diagonsl.
  * `a` specifies the first element of a 1D array which contains the elements of the diagonal band, see this. This array contains only the band elements and is obtained by running across the rows of the band matrix from left to right and top to bottom, starting at element _A_ 11. It must be dimensioned to contain at least `n*(l+1+r) - (r*(r+1)+l*(l+1)) div 2` elements. Note that a 2D array cannot be used for this routine.
  * `b` is the first element of the array containing the constant vector **b**. The array length at least must be equal to `n`. The vector will not be changed during the calculation.
  * `x` is the first element of the array to receive the solution vector **x**. It must be allocated to contain at least `n` values.
  * `ca` is a parameter to describe the accuracy of the solution.
  * `term` returns an error code: 
    * 1 - successful completion, the solution vector **x** is valid
    * 2 - the solution could not have been determined because the matrix is (almost) singular.
    * 3 - error in input values: `n < 1`, `l < 0`, `l >= n`, `r < 0`, or `r >= n`



  
**Example**

Solve this system of linear equations: 

    [math]\displaystyle{ \begin{array}{ccccccc} 5 x_1 & \- & 4 x_2 & \+ & x_3 & & & & & & & & & = 0 \\\ -4 x_1 & \+ & 6 x_2 & \- & 4 x_3 & \+ & x_4 & & & & & & & = 0 \\\ x_1 & \- & 4 x_2 & \+ & 6 x_3 & \- & 4 x_4 & & & & & & & = 0 \\\ & & x_2 & \- & 4 x_3 & \+ & 6 x_4 & \- & 4 x_5 & & & & & = 1 \\\ & & & & x_3 & \- & 4 x_4 & \+ & 6 x_5 & \- & 4 x_6 & \+ & x_7 & = 0 \\\ & & & & & & x_4 & \- & 4 x_5 & \+ & 6 x_6 & \- & 4 x_7 & = 0 \\\ & & & & & & & & x_5 & \- & 4 x_6 & \+ & 5 x_7 & = 0 \\\ \end{array} \qquad \Rightarrow \qquad A= \left[ \begin{array}{rrrr} 5 & -4 & 1 & 0 & 0 & 0 & 0 \\\ -4 & 6 & -4 & 1 & 0 & 0 & 0 \\\ 1 & -4 & 6 & -4 & 1 & 0 & 0 \\\ 0 & 1 & -4 & 6 & -4 & 1 & 0 \\\ 0 & 0 & 1 & -4 & 6 & -4 & 1 \\\ 0 & 0 & 0 & 1 & -4 & 6 & -4 \\\ 0 & 0 & 0 & 0 & 1 & -4 & 5 \end{array} \right] , \; \mathbf{b} = \left[ \begin{array}{r} 0 \\\ 0 \\\ 0 \\\ 1 \\\ 0 \\\ 0 \\\ 0 \end{array} \right] }[/math]
    
    
    program solve_band;
    
    uses
      typ, sle, omv;
    
    const
      n = 7;
      L = 2;
      R = 2;
      nelements = n * (L + 1 + R) - (L * (L + 1) + R * (R + 1)) div 2;
    
      // Band elements arranged in rows, elements outside of the band omitted!
      a: array[1..nelements] of Arbfloat = (
                 5, -4, 1,
             -4, 6, -4, 1,
          1, -4, 6, -4, 1,
          1, -4, 6, -4, 1,
          1, -4, 6, -4, 1,
          1, -4, 6, -4,
          1, -4, 5
        );
      b: array[1..n] of ArbFloat = (
        0, 0, 0, 1, 0, 0, 0
      );
    
    var
      x: array[1..n] of ArbFloat;
      a_test: array[1..n, 1..n] of ArbFloat;
      b_test: array[1..n] of ArbFloat;
      ca: ArbFloat;
      i, j, k: Integer;
      term: Integer;
    
    begin
      WriteLn('Solve matrix system A x = b where A is a band matrix');
      WriteLn;
    
      // Write diagonal elements
      WriteLn('Matrix A:');
      WriteLn('  n = ', n);
      Writeln('  Left band width L = ', L);
      WriteLn('  Right band width R = ', R);
      WriteLn('  Diagonal elements of A = ');
      for k := 1 to nelements do
        Write(a[k]:5:0);
      WriteLn;
      WriteLn;
    
      // Solve
      slegba(n, l, r, A[1], b[1], x[1], ca, term);
    
      if term = 1 then begin
        // To test the result we multiply A with the solution vector x and check
        // whether the result is equal to b.
        // Since mutliplication of a matrix stored like a band matrix is quite
        // cumbersome we copy the band matrix into a standard matrix.
        FillChar(a_test, SizeOf(a_test), 0);
        k := 1;
        for i:=1 to L do
          for j := i-L to i+R do
            if (j >= 1) and (j <= n) then begin
              a_test[i,j] := A[k];
              inc(k);
            end;
    
        for i:= L+1 to n-R do
          for j := i-L to i+R do begin
            a_test[i,j] := A[k];
            inc(k);
          end;
        for i := n-R+1 to n do
          for j := i-L to i+R do
            if j <= n then begin
              a_test[i,j] := A[k];
              inc(k);
            end;
    
        // Print A
        WriteLn('Matrix A =');
        for i:=1 to n do begin
          for j:=1 to n do
            Write(a_test[i,j]:5:0);
          WriteLn;
        end;
        WriteLn;
    
        // Print b
        WriteLn('Vector b = ');
        for i:= 1 to n do
          Write(b[i]:10:0);
        WriteLn;
        WriteLn;
    
        // Print solution
        WriteLn('Solution vector x = ');
        for i:= 1 to n do
          Write(x[i]:10:3);
        WriteLn;
        WriteLn;
    
        omvmmv(a_test[1,1], n, n, n, x[1], b_test[1]);
    
        WriteLn('Check result: A x = (must be equal to b)');
        for i:= 1 to n do
          Write(b_test[i]:10:0);
        WriteLn;
      end
      else
        WriteLn('Error');
    end.
    

  


### Symmetric positive definite band matrix

The optimized solution method for a symmetric positive band matrix is called `slegpb`. 
    
    
    procedure slegpb(n, w: ArbInt; var a, b, x, ca: ArbFloat; var term: ArbInt);
    

  * `n` denotes the number of columns and rows of the matrix (it must be square).
  * `w` identifies the (one-sided) bandwidth, i.e. the number of diagonals the band extends below (or to the left of) the main diagonal. This is the same value as the number of diagonals the band extends above (or to the right of) the main diagonsl.
  * `a` specifies the first element of a 1D array which contains the elements of the diagonal band, see this. This array contains only the band elements and is obtained by running across the rows of the band matrix from left to right and top to bottom, starting at element _A_ 11, but stopping at the main diagonal. This means that the array contains only the left band and the main diagonal elements. It must be dimensioned to contain at least `n*(w+1) - (w*(w+1)) div 2` elements. Note that a 2D array cannot be used for this routine.
  * `b` is the first element of the array containing the constant vector **b**. The array length at least must be equal to `n`. The vector will not be changed during the calculation.
  * `x` is the first element of the array to receive the solution vector **x**. It must be allocated to contain at least `n` values. If `term` is not 1 then the solution array contains undefined data.
  * `ca` is a parameter to describe the accuracy of the solution.
  * `term` returns an error code: 
    * 1 - successful completion, the solution vector **x** is valid
    * 2 - the solution could not have been determined because the matrix is (almost) singular.
    * 3 - error in input values: `n<1`, `w<0`, or `w>=n`



  
**Example**

The matrix of the band matrix example is symmetrical positive definite. Therefore, procedure `slegpb` can be applied as well: 
    
    
    program solve_spd;    // "spd" = symmetric positive definite
    
    uses
      typ, sle, omv;
    
    const
      n = 7;
      w = 2;
      nelements = n * (w + 1) - (w * (w + 1)) div 2;
    
      // Band elements arranged in rows, elements outside of the band as well as right diagonals omitted!
      a: array[1..nelements] of Arbfloat = (
                 5,
             -4, 6,
          1, -4, 6,
          1, -4, 6,
          1, -4, 6,
          1, -4, 6,
          1, -4, 5
        );
      b: array[1..n] of ArbFloat = (
        0, 0, 0, 1, 0, 0, 0
      );
    
    var
      x: array[1..n] of ArbFloat;
      a_test: array[1..n, 1..n] of ArbFloat;
      b_test: array[1..n] of ArbFloat;
      ca: ArbFloat;
      i, j, k: Integer;
      term: Integer;
    
    begin
      WriteLn('Solve matrix system A x = b where A is a symmetric positive definite band matrix');
      WriteLn;
    
      // Write diagonal elements
      WriteLn('Matrix A:');
      WriteLn('  n = ', n);
      Writeln('  (One-sided) band width w = ', w);
      WriteLn('  Diagonal elements of A = ');
      for k := 1 to nelements do
        Write(a[k]:5:0);
      WriteLn;
      WriteLn;
    
      // Solve
      slegpb(n, w, A[1], b[1], x[1], ca, term);
    
      if term = 1 then begin
        // To test the result we multiply A with the solution vector x and check
        // whether the result is equal to b.
        // Since mutliplication of a matrix stored like a band matrix is quite
        // cumbersome we copy the band matrix into a standard matrix.
        FillChar(a_test, SizeOf(a_test), 0);
        i := 1;
        k := 1;
        while (k <= nelements) and (i <= n) do begin
          for j := i - w to i do
            if (j >= 1) and (j <= n) then begin
              a_test[i, j] := a[k];
              a_test[j, i] := a[k];
              inc(k);
            end;
          inc(i);
        end;
    
        // Print A
        WriteLn('Matrix A =');
        for i:=1 to n do begin
          for j:=1 to n do
            Write(a_test[i,j]:5:0);
          WriteLn;
        end;
        WriteLn;
    
        // Print b
        WriteLn('Vector b = ');
        for i:= 1 to n do
          Write(b[i]:10:0);
        WriteLn;
        WriteLn;
    
        // Print solution
        WriteLn('Solution vector x = ');
        for i:= 1 to n do
          Write(x[i]:10:3);
        WriteLn;
        WriteLn;
    
        omvmmv(a_test[1,1], n, n, n, x[1], b_test[1]);
    
        WriteLn('Check result: A x = (must be equal to b)');
        for i:= 1 to n do
          Write(b_test[i]:10:3);
        WriteLn;
      end
      else
        WriteLn('Error');
    end.
    

  


### Tridiagonal matrix

NumLib offers two specialized procedures for solving a system of linear equations based on a tridiagonal matrix _A_. Both rely on the memory-optimized storage of tridiagonal matrices as described here. They differ in the internal calculation algorithm and in the behavior with critical matrices. 
    
    
    procedure sledtr(n: ArbInt; var l, d, u, b, x: ArbFloat; var term: ArbInt);
    procedure slegtr(n: ArbInt; var l, d, u, b, x, ca: ArbFloat; var term: ArbInt);
    

  * `n` is the number of unknown variables. It must be the same as the number of columns and rows of the coefficent matrix.
  * `l` specifies the first element in the subdiagonal of the matrix _A_. This 1D array must be dimensioned to at least `n-1` elements.
  * `d` specifies the first element along the main diagonal of the matrix _A_. This 1D array must be dimensioned to contain at least `n` elements.
  * `u` specifies the first element along the superdiagonal of the matrix _A_. The array must be dimensioned to have at least `n-1` elements.
  * `b` is the first element of the array containing the constant vector **b**. The array length at least must be equal to `n`. The vector will not be changed during the calculation.
  * `x` is the first element of the array to receive the solution vector **x**. It must be allocated to contain at least `n` values.
  * `ca` is a parameter to describe the accuracy of the solution.
  * `term` returns an error code: 
    * 1 - successful completion, the solution vector **x** is valid
    * 2 - the solution could not have been determined because the matrix is (almost) singular.
    * 3 - error in input values: `n < 1`.



`sledtr` is numerically stable if matrix _A_ fulfills one of these conditions: 

  * _A_ is regular (i.e. its inverse matrix exists), and _A_ is columnar-diagonally dominant; this means: 
    * |_d 1_| ≥ |_l_ 2|,
    * |_d i_| ≥ |_u_ _i_ -1| + |_l i+1|, _i _= 2, ...,_ n _-1,_
    * |_d n_| ≥ |_u_ _n_ -1|
  * _A_ is regular, and _A_ is diagonally dominant; this means: 
    * |_d 1_| ≥ |_l_ 2|,
    * |_d i_| ≥ |_u_ _i_ | + |_l i|, _i _= 2, ...,_ n _-1,_
    * |_d n_| ≥ |_u_ _n_ |
  * _A_ is symmetric and positive-definite.



However, this procedure does not provide the parameter `ca` from which the accuracy of the determined solution can be evaluated. If this is needed the (less stable) procedure `slegtr` must be used. 

  
**Example** Solve this tridiagonal system of linear equations for _n_ = 8: 

    [math]\displaystyle{ \begin{cases} 188 x_1 - 100 x_2 = 0 \\\ -100 x_1 + 188 x_2 -100 x_3 = 0 \\\ \vdots \\\ -100 x_{n-2} + 188 x_{n-1} - 100 x_n = 0 \\\ -100 x_{n-1} + 188 x_n = 0 \end{cases} \qquad \Rightarrow \qquad A= \left[ \begin{array}{rrrrr} 188 & -100 & & & 0 \\\ -100 & 188 & -100 & & \\\ & \ddots & \ddots & \ddots & \\\ & & -100 & 188 & -100 \\\ & & & -100 & 188 \\\ \end{array} \right] , \; \mathbf{b} = \left[ \begin{array}{r} 88 \\\ -12 \\\ \vdots \\\ -12 \\\ 88 \end{array} \right] }[/math]
    
    
    program solve_tridiag;
    
    uses
      typ, sle;
    
    const
      n = 8;
      u: array[1..n-1] of ArbFloat = (-100, -100, -100, -100, -100, -100, -100      );
      d: array[1..n] of ArbFloat   = ( 188,  188,  188,  188,  188,  188,  188,  188);
      l: array[2..n] of ArbFloat   = (      -100, -100, -100, -100, -100, -100, -100);
      b: array[1..n] of ArbFloat   = (  88,  -12,  -12,  -12,  -12,  -12,  -12,   88);
    
    var
      A: array[1..n, 1..n] of ArbFloat;
      x: array[1..n] of ArbFloat;
      b_test: array[1..n] of ArbFloat;
      ca: ArbFloat;
      i, j: Integer;
      term: Integer;
    
    begin
      WriteLn('Solve tridiagonal matrix system A x = b');
      WriteLn;
    
      Write('Superdiagonal of A:':25);
      for i := 1 to n-1 do
        Write(u[i]:10:0);
      WriteLn;
    
      Write('Main diagonal of A:':25);
      for i:= 1 to n do
        Write(d[i]:10:0);
      WriteLn;
    
      Write('Subdiagonal of A:':25);
      Write('':10);
      for i:=2 to n do
        Write(l[i]:10:0);
      WriteLn;
    
      Write('Vector b:':25);
      for i:=1 to n do
        Write(b[i]:10:0);
      WriteLn;
    
      // Solve
      slegtr(n, l[2], d[1], u[1], b[1], x[1], ca, term);
      { or:
      sledtr(n, l[2], d[1], u[1], b[1], x[1], term);
      }
    
      if term = 1 then begin
        Write('Solution vector x:':25);
        for i:= 1 to n do
          Write(x[i]:10:0);
        WriteLn;
    
        // Multiply A with x to test the result
        // NumLib does not have a routine to multiply a tridiagonal matrix with a
        // vector... Let's do it manually.
        b_test[1] := d[1]*x[1] + u[1]*x[2];
        for i := 2 to n-1 do
          b_test[i] := l[i]*x[i-1] + d[i]*x[i] + u[i]*x[i+1];
        b_test[n] := l[n]*x[n-1] + d[n]*x[n];
    
        Write('Check b = A x:':25);
        for i:= 1 to n do
          Write(b_test[i]:10:0);
        WriteLn;
      end
      else
        WriteLn('Error');
    end.
    

  


### Overdetermined systems (least squares)

Unlike the other routines in unit _sle_ which require a square matrix _A_ , `slegls` can solve linear systems described by a rectangular matrix which has more rows than colums, or speaking in terms of equations, has more equations than unknowns. Such a system generally is not solvable exactly. But an approximate solution can be found such that the sum of squared residuals of each equation, i.e. the norm [math]\displaystyle{ \|\mathbf{b} - A \mathbf{x}\|_2 }[/math] is as small as possible. The most prominent application of this technique is fitting of equations to data (regression analysis). 
    
    
    procedure slegls(var a: ArbFloat; m, n, rwidtha: ArbInt; var b, x: ArbFloat; var term: ArbInt);
    

  * `a` is the first (i.e. top left) element of an array with the coefficient matrix _A_ ; the other elements must follow within the same allocated memory block. The array will not be changed during the calculation.
  * `m` is the number of rows of the matrix _A_ (or: the number of equations).
  * `n` is the number of columns of the matrix _A_ (or: the number of unknown variables). n must not be larger than m.
  * `rwidth` identifies the allocated number of columns of the coefficient matrix _A_. This is useful if the matrix is allocated larger than needed. It is required that `n <= rwidth`.
  * `b` is the first element of the array containing the constant vector **b**. The array length must correspond to the number of matrix rows `m`, but the array can be allocated to be larger.
  * `x` is the first element of the array to receive the solution vector **x**. The array length must correspond to the number of matrix columns `n`, but the array can be allocated to be larger.
  * `term` returns an error code: 
    * 1 - successful completion, the solution vector **x** is valid
    * 2 - there is no unambiguous solution because the columns of the matrix are linearly dependant..
    * 3 - error in input values: `n < 1`, or `n` > `m`.



The method is based on reduction of the array _A_ to upper triangle shape through Householder transformations. 

  
**Example**

Find the least-squares solution for the system _A_ **x** = **b** of 4 equations and 3 unknowns with 

[math]\displaystyle{ A= \left( \begin{array}{rrr} 1 & 0 & 1 \\\ 1 & 1 & 1 \\\ 0 & 1 & 0 \\\ 1 & 1 & 0 \end{array} \right), \ \ b= \left( \begin{array}{r} 21 \\\ 39 \\\ 21 \\\ 30 \end{array} \right). }[/math]
    
    
    program solve_leastsquares;
    
    uses
      typ, sle, omv;
    
    const
      m = 4;
      n = 3;
    
      A: array[1..m, 1..n] of ArbFloat = (
        (1, 0, 1),
        (1, 1, 1),
        (0, 1, 0),
        (1, 1, 0)
      );
      b: array[1..m] of ArbFloat = (
        21, 39, 21, 30);
    
    var
      x: array[1..n] of ArbFloat;
      term: ArbInt;
      i, j: Integer;
      b_test: array[1..m] of ArbFloat;
      sum: ArbFloat;
    
    begin
      WriteLn('Solve A x = b with the least-squares method');
      WriteLn;
    
      // Display input data
      WriteLn('A = ');
      for i := 1 to m do
      begin
        for j := 1 to n do
          Write(A[i, j]:10:0);
        WriteLn;
      end;
      WriteLn;
      WriteLn('b = ');
      for i := 1 to m do
        Write(b[i]:10:0);
      WriteLn;
    
      // Calculate and show solution
      slegls(a[1,1], m, n, n, b[1], x[1], term);
    
      WriteLn;
      WriteLn('Solution x = ');
      for j:= 1 to n do
        Write(x[j]:10:0);
      WriteLn;
    
      // Calculate and display residuals
      WriteLn;
      WriteLn('Residuals A x - b = ');
      sum := 0;
      omvmmv(a[1,1], m, n, n, x[1], b_test[1]);
      for i:=1 to m do begin
        Write((b_test[i] - b[i]):10:0);
        sum := sum + sqr(b_test[i] - b[i]);
      end;
      WriteLn;
    
      // Sum of squared residuals
      WriteLn;
      WriteLn('Sum of squared residuals');
      WriteLn(sum:10:0);
    
      WriteLn;
      WriteLn('----------------------------------------------------------------------------');
      WriteLn;
    
      // Modify solution to show that the sum of squared residuals increases';
      WriteLn('Modified solution x'' (to show that it has a larger sum of squared residuals)');
      x[1] := x[1] + 1;
      x[2] := x[2] - 1;
      WriteLn;
      for j:=1 to n do
        Write(x[j]:10:0);
      omvmmv(a[1,1], m, n, n, x[1], b_test[1]);
      sum := 0;
      for i:=1 to m do
        sum := sum + sqr(b_test[i] - b[i]);
      WriteLn;
      WriteLn;
      WriteLn('Sum of squared residuals');
      WriteLn(sum:10:0);
    end.
    

## Eigenvalues and eigenvectors (unit _eig_)

A vector is an [eigenvector](<https://en.wikipedia.org/wiki/Eigenvalues_and_eigenvectors>) if it does not change its direction after a linear transformation has been applied to it. In matrix terms: if a non-zero vector **x** of dimension _n_ is multiplied to a square _n_ x _n_ matrix _A_ and the product is the same vector multiplied by a scalar _λ_ then **x** is an **eigenvector** , and _λ_ is called an **eigenvalue** : 

    [math]\displaystyle{ A \mathbf{x} = \lambda \mathbf{x} \qquad \Rightarrow \qquad A \mathbf{x} - \lambda \mathbf{x} = 0 }[/math]

NumLib offers a variety of routines for calculation of eigenvectors and eigenvalues. They are optimized for the matrix type. Every matrix type will be supported by two or four procedures of various degree of complexity; they are identified by appended numbers 1 and 2, or 1 to 4: 

  * The procedures with appended **1** calculate **all eigenvalues**.
  * The procedures with the appended **2** do the same but only for **some eigenvalues** which have an index in a specified range (_λ_ k1..._λ_ k2 out of _λ_ 1..._λ_ n).
  * The procedures with the appended **3** calculate **all eigenvalues and all eigenvectors**.
  * The procedures with the appended **4** do the same, but again return only **some eigenvalues and eigenvectors** having an index in the specified interval.



  


### Matrices with general storage

The routines discussed here assume that the _n_ x _n_ matrix for which the eigenvalues are to be calculated is stored in the conventional way as a 2D (or 1D) array of `ArbFloat` values. 

The NumLib method for determining the eigenvalues and eigenvectors of a generic matrix are called `eigge1` and `eigge3` ("ge" = "_generic_ "). Optimized calculation schemes exists for generic symmetric matrices and symmetric positive definite matrices; the corresponding routines are called `eiggs1`, `eiggs2`, `eiggs3`, and `eiggs4` for generic symmetric matrices ("gs" = "generic symmetric")., and `eiggg1`, `eiggg2`, `eiggg3`, `eiggg4` for symmetric positive definite matrices ("gg" = ??) 
    
    
    // Generic matrix (without any special symmetries)
    procedure eigge1(var a: ArbFloat; n, rwidth: ArbInt; var lam: complex; var term: ArbInt);                      // all eigenvalues
    procedure eigge3(var a: ArbFloat; n, rwidtha: ArbInt; var lam, x: complex; rwidthx: ArbInt; var term: ArbInt); // all eigenvalues and eigenvectors
    
    // Generic symmetric matrix
    procedure eiggs1(var a: ArbFloat; n, rwidth: ArbInt; var lam: ArbFloat; var term: ArbInt);                     // all eigenvalues
    procedure eiggs2(var a: ArbFloat; n, rwidth, k1, k2: ArbInt; var lam: ArbFloat; var term: ArbInt);             // some eigenvalues (index k1..k2)
    procedure eiggs3(var a: ArbFloat; n, rwidtha: ArbInt; var lam, x: ArbFloat; var term: ArbInt);                 // all eigenvalues and eigenvectors
    procedure eiggs4(var a: ArbFloat; n, rwidtha, k1, k2: ArbInt; var lam, x: ArbFloat; var term: ArbInt);         // some eigenvalues and eigenvectors (index k1..k2)
    
    // Symmetric positive definite matrix
    procedure eiggg1(var a: ArbFloat; n, rwidth: ArbInt; var lam: ArbFloat; var term: ArbInt);                     // all eigenvalues
    procedure eiggg2(var a: ArbFloat; n, rwidth, k1, k2: ArbInt; var lam: ArbFloat; var term: ArbInt);             // some eigenvalues (index k1..k2)
    procedure eiggg3(var a: ArbFloat; n, rwidtha: ArbInt; var lam, x: ArbFloat; var term: ArbInt);                 // all eigenvalues and eigenvectors
    procedure eiggg4(var a: ArbFloat; n, rwidtha, k1, k2: ArbInt; var lam, x: ArbFloat; var term: ArbInt);         // some eigenvalues and eigenvectors (index k1..k2)
    

  * `a` is the first element of an array containing the matrix _A_ for which the eigenvalue/eigenvector has to be calculated. The array must be dimensioned to provide space for at least _n_ 2 floating point values.
  * `n` specifies the size of the matrix _A_ , i.e. the number of rows or columns. Note that the input matrix must be square, i.e. the number of rows and columns is equal.
  * `rwidth` is the allocated row length of the array `a`. It can be larger than _n_ if the array is allocated larger than necessary, but normally `rwidth = n`.
  * `lam` is the first element of an array receiving the calculated eigenvalues. In case of a generic matrix (`eigge1`, `eigge2`) the eigenvalues can be complex; therefore, the array must be dimensioned for values of type `complex` as declared in unit _typ_. In the other cases (`eiggs1..4` or `eiggg1..4`) the eigenvalues are real, and the array must be dimensioned for datatype `ArbFloat`. Since a _n_ x _n_ matrix has _n_ eigenvalues the array must be allocated for at least `n` values, in case of the procedures with appended 2 or 4 only `k2-k1+1` values are sufficient (see below).
  * `term` returns an error code: 
    * 1 -- successful calculation
    * 2 -- calculation failed
    * 3 -- error in input data: `n<1`, `k1<1`, `k1>k2`, or `k2>n`.



Additionally, in case of procedure `eigge3`, `eigge4` , `eiggg3` or `eiggg4`: 

  * `x` is the first element of a matrix to receive the calculated eigenvectors. Again, the eigenvectors of a generic matrix can have complex components. Therefore, the matrix must be declared for the datatype `complex`, and it must be large enough to hold at least _n_ 2 values, in case of `eigge4` or `eiggg4` `n * (k2-k1+1)`. If the matrix is symmetric or positive definite, the eigenvectors are real, and the array must be declared for datatype `ArbFloat`. In any case, the eigenvectors are normalized to unit length and arranged in the columns of this matrix.
  * `rwidthx` denotes the allocated row length of the matrix `x`. Thus it is possible to dimension the result matrix larger than actually needed.



Additionally, in case of procedures `eiggs2`, `eiggs4`, `eiggg2` and `eiggg4`: 

  * `k1` and `k2` define the interval of indexes _k_ for which the eigenvalues (_λ k_) and eigenvalues (**c k**) are to be calculated. They are integers and must be ordered such that `1<=k1<=k2<=n`.



  
**Example**

Calculate the eigenvalues and eigenvectors of the matrix 

    [math]\displaystyle{ A= \begin{bmatrix} 8 & -1 & -5 \\\ -4 & 4 & -2 \\\ 18 & -5 & -7 \end{bmatrix} }[/math]
    
    
    program eig_general_matrix;
    
    uses
      SysUtils, math,
      typ, eig;
    
    const
      n = 3;
      D = 3;
    
    var
      // a is the input matrix
      a: array[1..n, 1..n] of ArbFloat = (
        ( 8, -1, -5),
        (-4,  4, -2),
        (18, -5, -7)
      );
      lambda: array[1..n] of complex;
      x: array[1..n, 1..n] of complex;
      term: Integer = 0;
      i, j: Integer;
    
      function ComplexToStr(z: complex; Decimals: Integer): String;
      var
        sgn: array[boolean] of string = ('+', '-');
      begin
        Result := Format('%.*f %s %.*fi', [Decimals, z.Re, sgn[z.Im < 0], Decimals, abs(z.Im)]);
      end;
    
    begin
      // write input matrix
      WriteLn('a = ');
      for i := 1 to n do begin
        for j := 1 to n do
          Write(a[i, j]:10:D);
        WriteLn;
      end;
      WriteLn;
    
      // Calculate eigenvalues/vectors
      eigge3(a[1,1], n, n, lambda[1], x[1,1], n, term);
    
      // write eigenvalues
      WriteLn('Eigenvalues: lambda = ');
      for i := 1 to n do
        Write(ComplexToStr(lambda[i], D):25);
      WriteLn;
      WriteLn;
    
      // Write eigenvectors
      WriteLn('Eigenvectors (as columns): x = ');
      for i := 1 to n do begin
        for j := 1 to n do
          Write(ComplexToStr(x[i, j], D):25);
        WriteLn;
      end;
    end.
    

  


### Symmetric band matrices

NumLib provides four routines to calculate eigenvalues and eigenvectors of symmetric band matrices: `eigbs1`...`eigbs4` ("bs" = _band_ and _symmetric_): 
    
    
    // All eigenvalues
    procedure eigbs1(var a: ArbFloat; n, w: ArbInt; var lam: ArbFloat; var term: ArbInt);
    
    // Some eigenvalues (index k1..k2)
    procedure eigbs2(var a: ArbFloat; n, w, k1, k2: ArbInt; var lam: ArbFloat; var term: ArbInt);
    
    // All eigenvalues and all eigenvectors
    procedure eigbs3(var a: ArbFloat; n, w: ArbInt; var lam, x: ArbFloat; rwidthx: ArbInt; var term: ArbInt);
    
    // Some eigenvalues, some eigenvectors (index k1..k2)
    procedure eigbs4(var a: ArbFloat; n, w, k1, k2: ArbInt; var lam, x: ArbFloat; rwidthx: ArbInt; var m2, term: ArbInt);
    

  * `a` specifies the first element of a 1D array which contains the diagonal elements of the left band and the main diagonal. It is obtained by running across the rows of the band matrix from left to right and top to bottom, but stopping at the main diagonal - the right band is ignored since it is equal to the left band. The array must be dimensioned to contain at least `n`*(`w`+1) - (`w`*(`w`+1)) div 2 elements.
  * `n` denotes the number of columns and rows of the matrix (it must be square).
  * `w` identifies the bandwidth, i.e. how many diagonals the band extends to the left or to the right of the main diagonal. Left and right bandwidth must be equal because the matrix is symmetric.
  * `lam` is the first element of a real-valued array receiving the calculated eigenvalues. The eigenvalues of a symmetric matric are real values. Since an _n_ x _n_ matrix has _n_ eigenvalues the array must be allocated for at least _n_ `ArbFloat` values.
  * `x`, in case of routines `eigbs3` and `eigbs4`, is the first element of a matrix to receive the calculated eigenvectors. The matrix must be declared for the datatype `Arbfloat`, and it must be large enough to hold at least _n_ x _n_ values. The eigenvectors are normalized to unit length and are arranged in the columns of this matrix.
  * `rwidthx`, in case of routines `eigbs3` and `eigbs4`, denotes the allocated row length of the matrix **x**. Thus it is possible to dimension the result matrix larger than actually needed.
  * `k1` and `k2`, in case of procedures `eigbs2` and `eigbs4`, define the interval of indexes _k_ for which the eigenvalues λk and eigenvalues **x** k are to be calculated. They are integers and must be ordered such that `1<=k1<=k2<=n`.
  * `m2`, in case of routine `eigbs4`, indicates the index of the largest eigenvalue for which an eigenvector has been calculated.
  * `term` returns an error code: 
    * 1 -- successful calculation
    * 2 -- calculation failed
    * 3 -- error in input parameters: `n<1`, `w<0`, `w>=n`, `k1<1`, `k1>k2`, or `k2>n`.



If the bandwidth `w > n/3` then it is better to calculate all eigenvalues and eigenvectors even if not all of them are needed. 

  
**Example**

Calculate the eigenvalues and eigenvectors of the symmetric 7 x 7 matrix [math]\displaystyle{ A= \begin{bmatrix} 5 & -4 & 1 & & & & 0 \\\ -4 & 6 & -4 & 1 & & & \\\ 1 & -4 & 6 & -4 & 1 & & \\\ & 1 & -4 & 6 & -4 & 1 & \\\ & & 1 & -4 & 6 & -4 & 1 \\\ & & & 1 & -4 & 6 & -4 \\\ 0 & & & & 1 & -4 & 5 \end{bmatrix} }[/math] The eigenvalues are [math]\displaystyle{ \lambda_k = 16\sin^{4}\frac{k\pi}{16}, \qquad k=1,2,\ldots,7 }[/math]
    
    
    program eig_symmband_matrix;
    
    uses
      SysUtils, math,
      typ, eig;
    
    const
      n = 7;
      w = 2;
      nelements = n * (w + 1) - (w * (w + 1)) div 2;
      D = 3;
    
      // a contains the elements of the left and main diagonals of the input matrix
      a: array[1..nelements] of ArbFloat = (
         5,
        -4,  6,
         1, -4,  6,
             1, -4,  6,
                 1, -4,  6,
                     1, -4, 6,
                         1, -4, 5 );
    
      function MatrixIndexToArrayIndex(i, j, n, w: Integer): Integer;
    
        function DiagElementIndex(i: Integer): Integer;
        var
          k: Integer;
        begin
          Result := 1;
          if i = 1 then
            exit;
          // Rows truncated at left
          for k := 2 to w do begin
            Result := Result + k;
            if k = i then
              exit;
          end;
          // full rows and those truncated at right
          for k := w+1 to n do begin
            Result := Result + w + 1;
            if k = i then
              exit;
          end;
          Result := n;
        end;
    
      var
        d: Integer;
      begin
        if j > i then begin
          Result := MatrixIndexToArrayIndex(j, i, n, w);
          exit;
        end;
    
        Result := DiagElementIndex(i);
        if (i = j) then
          exit;
    
        d := i - j;
        if d > w then
          Result := -1
        else begin
          dec(Result, d);
          if (Result < 1) then
            Result := -1
        end;
      end;
    
    var
      lambda: array[1..n] of ArbFloat;
      x: array[1..n, 1..n] of ArbFloat;
      term: Integer = 0;
      i, j, k: Integer;
    
    begin
      // Write diagonal elements
      WriteLn('n = ', n);
      Writeln('(One-sided) band width w = ', w);
      Write('Diagonal elements of A = ', a[1]:0:0);
      for k := 2 to nelements do
        Write(a[k]:3:0);
      WriteLn;
      WriteLn;
    
      // write reconstructed band input matrix (not needed for calculation)
      WriteLn('Reconstructed A = ');
      k := 1;
      for i := 1 to n do begin
        for j := 1 to n do begin
          k := MatrixIndexToArrayIndex(i, j, n, w);
          if k = -1 then
            Write(0.0:3:0)
          else
            Write(a[k]:3:0);
        end;
        WriteLn;
      end;
      WriteLn;
    
      // Calculate eigenvalues/vectors
      eigbs3(a[1], n, w, lambda[1], x[1,1], n, term);
      if term <> 1 then begin
        WriteLn('term = ', term, ' --> ERROR');
        halt;
      end;
    
      // Write expected results of eigenvalues
      WriteLn('Expected eigenvalues:');
      for i := 1 to n do
        Write(16 * intpower(sin(i*pi/16), 4):15:D);
      WriteLn;
      WriteLn;
    
      // write eigenvalues
      WriteLn('Calculated eigenvalues: lambda = ');
      for i := 1 to n do
        Write(lambda[i]:15:D);
      WriteLn;
      WriteLn;
    
      // Write eigenvectors
      WriteLn('Eigenvectors (as columns): x = ');
      for i := 1 to n do begin
        for j := 1 to n do
          Write(x[i, j]:15:D);
        WriteLn;
      end;
    end.
    

  


### Symmetric tridiagonal matrices

This type of matrices is treated best by the routines `eigts1`..`eigts4`: 
    
    
    // All eigenvalues
    procedure eigts1(var d, cd: ArbFloat; n: ArbInt; var lam: ArbFloat; var term: ArbInt);
    
    // Some eigenvalues (with indices k1..k2)
    procedure eigts2(var d, cd: ArbFloat; n, k1, k2: ArbInt; var lam: ArbFloat; var term: ArbInt);
    
    // All eigenvalues and all eigenvectors
    procedure eigts3(var d, cd: ArbFloat; n: ArbInt; var lam, x: ArbFloat; rwidth: ArbInt; var term: ArbInt);
    
    // Some eigenvalues and eigenvectors (with indices k1..k2)
    procedure eigts4(var d, cd: ArbFloat; n, k1, k2: ArbInt; var lam, x: ArbFloat; rwidth: ArbInt; var m2, term: ArbInt);
    

  * `d` specifies the first element along the main diagonal of the matrix. This 1D array must be dimensioned to contain at least `n` elements.
  * `c` specifies the first element in the subdiagonal of the matrix. This 1D array must be dimensioned to at least `n-1` elements.
  * `n` denotes the number of columns or rows of the matrix (it must be square).
  * `k1` and `k2` define the interval of indexes _k_ for which the eigenvalues (_λ k_) and eigenvalues (**c k**) are to be calculated. They are integers and must be ordered such that `1<=k1<=k2<=n`.
  * `lam` is the first element of an array receiving the calculated eigenvalues. Since there are _n_ real eigenvalues the array must be dimensioned to contain at least _n_ values of type `ArbFloat`. In case of the routines with appended 2 and 4, it is sufficient to provide space only for `k2 - k1 + 1` values.
  * `x` is the first element of an array to receive the calculated eigenvectors. Since the eigenvectors are real the array must be prepared for data type `ArbFloat`. In case of routine `eigts3`, the array must be dimensioned for at least _n_ 2 elements, in case of routine `eigts4` it must provide space for at least `n * (k2-k1+1)` values. The eigenvectors are normalized to unit length and arranged in the columns of this matrix.
  * `rwidthx` denotes the allocated row length of the matrix `x`. Thus it is possible to dimension the result matrix larger than actually needed.
  * `term` returns an error code: 
    * 1 -- successful calculation
    * 2 -- calculation failed
    * 3 -- error in input data: `n<1`, `k1<1`, `k1>k2`, or `k2>n`.



  
**Example**

Calculate eigenvalues and eigenvectors of the matrix [math]\displaystyle{ A= \begin{bmatrix} 1 & 1 & 0 & 0 \\\ 1 & 1 & 2 & 0 \\\ 0 & 2 & 1 & 1 \\\ 0 & 0 & 1 & 1 \end{bmatrix} }[/math] The expected eigenvalues are [math]\displaystyle{ -\sqrt{2},\ 2-\sqrt{2},\ \sqrt{2},\ 2+\sqrt{2} }[/math]
    
    
    program eig_symmtridiag_matrix;
    
    uses
      typ, eig;
    
    const
      n = 4;
    
      // a contains the elements of the main diagonal of the input matrix
      d: array[1..n] of ArbFloat = (1, 1, 1, 1);
      // c contains the elements of the subdiagonal
      c: array[2..n] of ArbFloat = (1, 2, 1);
    
    var
      lambda: array[1..n] of ArbFloat;
      x: array[1..n, 1..n] of ArbFloat;
      term: Integer = 0;
      i, j, k: Integer;
    
    begin
      // Write diagonals elements
      WriteLn('n = ', n);
      Write('Elements of main diagonal = ', d[1]:0:0);
      for k := 2 to n do
        Write(d[k]:3:0);
      WriteLn;
      Write('Elements of subdiagonal   = ', ' ':3, c[2]:0:0);
      for k := 3 to n do
        Write(c[k]:3:0);
      WriteLn;
      WriteLn;
    
      // write reconstructed band input matrix (not needed for calculation)
      WriteLn('Reconstructed A = ');
      for i := 1 to n do begin
        for j := 1 to n do begin
          if j = i then
            Write(d[i]:3:0)
          else if (j = i-1) then
            Write(c[i]:3:0)
          else if (j = i+1) then
            Write(c[i+1]:3:0)
          else
            Write(0.0:3:0);
        end;
        WriteLn;
      end;
      WriteLn;
    
      // Calculate eigenvalues/vectors
      eigts3(d[1], c[2], n, lambda[1], x[1,1], n, term);
      if term <> 1 then begin
        WriteLn('term = ', term, ' --> ERROR');
        halt;
      end;
    
      // Write expected results of eigenvalues
      WriteLn('Expected eigenvalues:');
      Write(-sqrt(2):15:3, 2-sqrt(2):15:3, sqrt(2):15:3, 2+sqrt(2):15:3);
      WriteLn;
      WriteLn;
    
      // write eigenvalues
      WriteLn('Calculated eigenvalues: lambda = ');
      for i := 1 to n do
        Write(lambda[i]:15:3);
      WriteLn;
      WriteLn;
    
      // Write eigenvectors
      WriteLn('Eigenvectors (as columns): x = ');
      for i := 1 to n do begin
        for j := 1 to n do
          Write(x[i, j]:15:3);
        WriteLn;
      end;
    end.
    

  


## Finding the roots of a function (unit _roo_)

The roots are the _x_ values at which a function _f(x)_ is zero. 

  


### Roots of a polynomial

A polynomial of degree _n_

    [math]\displaystyle{ z^n + a_1 z^{n-1} + a_2 z^{n-2} + ... + a_{n-1} z + a_n = 0 }[/math]

always has _n_ , not necessarily distinct, complex solutions. The datatype `complex` has been described in section complex numbers. These roots can be calculated by the function `roopol`: 
    
    
    procedure roopol(var a: ArbFloat; n: ArbInt; var z: complex; var k, term: ArbInt);
    

  * `a` is the first element of an array containing the polynomial coefficients. Note that it is assumed that the polynomial has been normalized such that the coefficient of the highest-degree term is 1; this coefficient is not contained in the array. Therefore, the array must be dimensioned for at least _n_ values. Since only real polynomials are handled here the array elements must be of datatype `ArbFloat`. The polynomial coefficients must be ordered from highest to lowest degree. Note that the data organization of the array is **different** from other polynomial routines in this library.
  * `n` is the degree of the polynomial. Must be a positive integer.
  * `z` is the first element in an array of `complex` values which returns the roots of the polynomial. The array must be dimensioned to contain at least _n_ values. The returned values are undefined if an error had occurred (`term` <> 1).
  * `k` returns how many roots were found. Always must be equal to _n_ , otherwise an error has occured.
  * `term` returns an error code: 
    * 1 -- sucessful completion, the result array contains valid data.
    * 2 -- Not all roots could be detected, i.e. `k < n`.
    * 3 -- Error in input data: `n < 1`.



  
**Example**

Calculate the roots of the polynomial _z_ 5 \+ 3 _z_ 4 \+ 4 _z_ 3 \- 8 _z_ 2. The expected zero points are: 

    [math]\displaystyle{ z_1=0,\ z_2=0,\ z_3=1,\ z_4=-2+2i,\ z_5=-2-2i }[/math]
    
    
    program polynomial;
    
    uses
      SysUtils, typ, roo;
    
    const
      n = 5;
      a: array[1..n] of ArbFloat = (3, 4, -8, 0, 0);
    
    var
      z: array[1..n] of complex;
      k: ArbInt;
      term: ArbInt;
      i: Integer;
      c: complex;
    
      function ComplexToStr(z: complex; Decimals: Integer): string;
      const
        SIGN: array[boolean] of string = ('+', '-');
      begin
        Result := Format('%.*f %s %.*f i', [Decimals, z.re, SIGN[z.im <0], Decimals, abs(z.im)]);
      end;
    
    begin
      // Solve equation
      roopol(a[1], n, z[1], k, term);
    
      if term = 1 then begin
        // Display results
        WriteLn('Results of procedure roopol:');
        for i:=1 to n do
          WriteLn('  Solution #', i, ': ', ComplexToStr(z[i], 6):20);
        WriteLn;
    
        // Display expected results
        Writeln('Expected results:');
        c.Init(0, 0);  // z1 = 0
        WriteLn('  Solution #1: ', complexToStr(c, 6):20);
        c.Init(0, 0);  // z2 = 0
        WriteLn('  Solution #2: ', complexToStr(c, 6):20);
        c.Init(1, 0);  // z3 = 1
        WriteLn('  Solution #3: ', complexToStr(c, 6):20);
        c.Init(-2, +2);  // z4 = -2 + 2 i
        WriteLn('  Solution #4: ', complexToStr(c, 6):20);
        c.Init(-2, -2);  // z4 = -2 - 2 i
        WriteLn('  Solution #5: ', complexToStr(c, 6):20);
      end else
        WriteLn('ERROR');
    end.
    

  


### Roots of the quadratic equation

The quadratic equation 

    [math]\displaystyle{ {z}^2 + {p} {z} + {q} = 0 }[/math]

is a special polynomal of degree 2. It always has two, not necessarily different, complex roots. These solutions can be determined by the procedure `rooqua`. 
    
    
    procedure rooqua(p, q: ArbFloat; var z1, z2: complex);
    

  * `p` and `q` are the (real) coefficients of the quadratic equation
  * `z1` and `z2` return the two complex roots. See unit _typ_ for the declaration of the type `complex`



Note that `rooqua` assumes that the coefficient of the quadratic term has been normalized to be 1. 

**Example**

Determine the roots of the equation _z 2 \+ 2 z + 5 = 0_. 
    
    
    program quadratic_equ;
    
    uses
      SysUtils, typ, roo;
    
    var
      z1, z2: complex;
    
    const
      SIGN: array[boolean] of string = ('+', '-');
    
    begin
      rooqua(2, 5, z1, z2);
    
      WriteLn(Format('1st solution: %g %s %g i', [z1.re, SIGN[z1.im < 0], abs(z1.im)]));
      WriteLn(Format('2nd solution: %g %s %g i', [z2.re, SIGN[z2.im < 0], abs(z2.im)]));
    end.
    

  


### Roots of the binomial equation

The equation 

    [math]\displaystyle{ z^n = a }[/math]

is another special polynomial in which all terms except for the highest- and lowest-order terms have zero coefficients. It has _n_ complex solutions which can be calculated by the procedure `roobin`: 
    
    
    procedure roobin(n: ArbInt; a: complex; var z: complex; var term: ArbInt);
    

  * `n` is the exponent in the binomial equation used. _n_ must be a positive integer.
  * `a` is the constant term _a_ at the right side of the binomial equation. It is expected to be a complex value (see section on complex numbers).
  * `z` is the first element of an array of data type `complex` to receive the results of the procedure. It must be dimensioned for at least _n_ `complex` values.
  * `term` returns an error code: 
    * 1 -- successful termination
    * 2 -- error in input data: `n < 1`



  
**Example**

Calculate the roots of the equation 

    [math]\displaystyle{ z^4 = -1 }[/math]

The exact solutions are 

    [math]\displaystyle{ z_1 = \frac{1}{2} \sqrt{2} (1+i) ,\ z_2 = \frac{1}{2} \sqrt{2} (1-i) ,\ z_3= \frac{1}{2} \sqrt{2} (-1+i) ,\ z_4= \frac{1}{2} \sqrt{2} (-1-i) }[/math]
    
    
    program binomial;
    
    uses
      SysUtils, typ, roo;
    
    const
      n = 4;
    
    var
      z: array[1..n] of complex;
      term: ArbInt;
      i: Integer;
      a: complex;
      c: complex;
    
      function ComplexToStr(z: complex; Decimals: Integer): string;
      const
        SIGN: array[boolean] of string = ('+', '-');
      begin
        Result := Format('%.*g %s %.*g i', [Decimals, z.re, SIGN[z.im < 0], Decimals, abs(z.im)]);
      end;
    
    begin
      // Prepare constant term as a complex value
      a.Init(-1, 0);
    
      // Solve equation
      roobin(n, a, z[1], term);
    
      if term = 1 then begin
        // Display results
        WriteLn('Results of procedure roobin:');
        for i:=1 to n do
          WriteLn('  Solution #', i, ': ', ComplexToStr(z[i], 6):20);
        WriteLn;
    
        // Display expected results
        Writeln('Expected results:');
        c.Init(1, 1);
        c.Scale(0.5*sqrt(2));
        WriteLn('  Solution #1: ', complexToStr(c, 6):20);
        c.Init(1, -1);
        c.Scale(0.5*sqrt(2));
        WriteLn('  Solution #2: ', complexToStr(c, 6):20);
        c.Init(-1, 1);
        c.Scale(0.5*sqrt(2));
        WriteLn('  Solution #3: ', complexToStr(c, 6):20);
        c.Init(-1, -1);
        c.Scale(0.5*sqrt(2));
        WriteLn('  Solution #4: ', complexToStr(c, 6):20);
      end else
        WriteLn('ERROR');
    end.
    

  


### Bisection

In the [bisection method](<https://en.wikipedia.org/wiki/Bisection_method>) two _x_ values _a_ and _b_ are estimated to be around the expected root such that the function values have opposite signs at _a_ and _b_. The center point of the interval is determined, and the subinterval for which the function has opposite signs at its endpoints is selected for a new iteration. The process ends when a given precision, i.e. interval length, is achieved. 

In NumLib, this approach is supported by the procedure `roof1r`: 
    
    
    procedure roof1r(f: rfunc1r; a, b, ae, re: ArbFloat; var x: ArbFloat; var term: ArbInt);
    

  * `f` is the function for which the root is to be determined. It must be a function of one floating point argument (type `ArbFloat`). The type of the function, `rfunc1r`, is declared in unit `typ`.
  * `a` and `b` are the endpoints of the test interval. The root must be located between these two values, i. e. the function values _f(a)_ and _f(b)_ must have different signs.
  * `ae` and `re` determine the absolute and relative precision, respectively, with which the root will be determined. `re` is relative to the maximum of _abs(a)_ and _abs(b)_. Note that precision and speed are conflicting issues. Highest accuracy is achieved if `ae` is given as `MachEps` (see unit `typ`). Both parameters must not be negative.
  * `x` returns the value of the found root.
  * `term` returns whether the process has been successful: 
    * 1 - successful termination, a zero point has been found with absolute accuracy `ae` or relative accuracy `re`
    * 2 - the required accuracy of the root could not be reached; However, the value of x is called the "best achievable" approach
    * 3 - error in input parameters: `ae` < 0 or `re` < 0, or _f(a)_ *_f(b)_ > 0



**Example**

The following program determines the square root of 2. This is the _x_ value at which the function _f(x)_ = _x_ ^2 - 2 is zero. Since _f(1)_ = 1^2 - 2 = -1 < 0 and _f(2)_ = 2^2 - 2 = 2 > 0 we can assume _a_ and _b_ to be 1 and 2, respectively. 
    
    
    program bisection_demo;
    
    uses
      typ, roo;
    
    function f(x: ArbFloat): ArbFloat;
    begin
      Result := x*x - 2;
    end;
    
    var
      x: ArbFloat = 0.0;
      term : ArbInt;
    
    begin
      roof1r(@f, 1.0, 2.0, 1e-9, 0, x, term);
      WriteLn('Bisection result ', x);
      WriteLn('sqrt(2)          ', sqrt(2.0));
    end.
    

  


### Roots of a system of nonlinear equations

Procedure `roofnr` finds the roots of a system of (nonlinear) equations: 

    [math]\displaystyle{ f_{i}(x_1,x_2,\ldots,x_n)=0, \; i=1,2,\ldots,n }[/math]
    
    
    procedure roofnr(f: roofnrfunc; n: ArbInt; var x, residu: ArbFloat; ra: ArbFloat; var term: ArbInt);
    

  * `f` contains the address of the procedure that calculates the function values _f_ _i_(_x_ 1, _x_ 2, ..., _x_ _n_). The type of the function, `roofnrfunc`, is declared in unit _typ_ to be


    
    
      type
        roofnrfunc = procedure(var x, fx: ArbFloat; var deff: boolean);
    

    

  * Here, `x` is the first element of an array of at least _n_ `ArbFloat` values which provides the xj values at which the functions are to be calculated.
  * `fx` is the first element of another array of _n_ `ArbFloat` values to receive the result of each function _f_ _i_. Suppose, we want to solve the following system of equations


    [math]\displaystyle{ \begin{array}{l} f_1(x_1, x_2) = x_1^2 + x_2^2 - 2 = 0 \\\ f_2(x_1, x_2) = -(x_1 - 1)^2 + x_2 = 0 \end{array} }[/math]

    Then the function to be used as `f` can be written like this:
    
    
        procedure func(var x1, f1x: real; var deff: boolean); 
        var 
          x: array[1..2] of real absolute x1;
          f: array[1..2] of real absolute f1x;
        begin
          f[1] := sqr(x[1]) + sqr(x[2]) - 2;
          f[2] := -sqr(x[1] - 1) + x[2];
        end;
    

    

  * The boolean parameter `deff` provides a way to stop the root finding process if some condition is met. The above example has two solutions, namely the intersection of a circle with a parabola. If only the intersection with `x[1] >= 1` is requested then the process can be stopped as soon as a value `x[1] < 1` is reached by setting `deff` to `false`:


    
    
        procedure func(var x1, f1x: real; var deff: boolean); far;
        var 
          x: array[1..2] of real absolute x1;
          f: array[1..2] of real absolute f1x;
        begin
          deff := x[1] >= 1;
          if deff then begin
            f[1] := sqr(x[1]) + sqr(x[2]) - 2;
            f[2] := -sqr(x[1] - 1) + x[2];
          end;
        end;
    

  * `n` is the number of equations or independent x values (_x_ i) in the system. Both must be the same.
  * `x` is the first element of an array of at least _n_ `ArbFloat` values which serves for both input and output. On **input** the array must contain estimates of the roots to be used as starting points of the iterative calculation. On **output** , the array is overwritten with the zero points found. Inspect the returned value of `term` to decide whether the output array contains a valid solution.
  * `residu` is the 2-norm of the vector of residuals in the calculated solution



    [math]\displaystyle{ \|f\|_2 = \sqrt{\Sigma_{i=1}^{n} f_i^2} }[/math]

  * `ra` contains the relative accuracy with which the desired solution is to be calculated. Recommended values: 10-3, 10-5, 10-8 if `ArbFloat` is `single`, `real`, or `double`.
  * `term` returns an error code: 
    * 1 -- successfull completion, a solution has been found with the desired accuracy
    * 2 -- the desired accuracy could not be reached, the returned values of _x_ are the best ones achievable.
    * 3 -- Incorrect input data: `n < 0`, or `re < 0`
    * 4 -- The calculation process has been stopped because an internal limit for the count of function calls has been reached. Retry with other starting values for x.
    * 5 -- Not enough progess, there may be no solution at all. If the solution is close to 0 choose another starting value.
    * 6 -- The procedure wants to calculate a value outside the range defined by `deff`.



  
**Example**

Calculate a solutions of the equation system 

    [math]\displaystyle{ \begin{array}{l} f_1(x_1, x_2) = x_1^2 + x_2^2 - 2 = 0 \\\ f_2(x_1, x_2) = -(x_1 - 1)^2 + x_2 = 0 \end{array} }[/math]
    
    
    program nonlinsystem;
    
    uses
      SysUtils, typ, roo;
    
    const
      n = 2;
      ra = 1e-10;
    
    var
      x: array[1..n] of ArbFloat;
      f_check: array[1..n] of ArbFloat;
      residu: ArbFloat;
      i: Integer;
      term: Integer;
      deff: Boolean;
    
      procedure funcs(var x0, fx: ArbFloat; var deff: boolean);
      var
        xloc: array[1..n] of ArbFloat absolute x0;
        f: array[1..n] of ArbFloat absolute fx;
        x, y, z: ArbFloat;
      begin
        x := xloc[1];
        y := xloc[2];
        f[1] :=  sqr(x) + sqr(y) - 2;
        f[2] := -sqr(x - 1) + y;
      end;
    
    begin
      // Initial guessed values
      x[1] := 0;
      x[2] := 0;
    
      // Solve equation
      roofnr(@funcs, n, x[1], residu, ra, term);
    
      WriteLn('term = ', term);
      WriteLn;
    
      if term in [1, 2] then begin
        // Display results
        WriteLn('Results found by procedure roofnr:');
        for i:=1 to n do
          WriteLn('Solution #' + IntToStr(i)+': ':20, x[i]:0:6);
        WriteLn;
    
        WriteLn('Norm of residuals: ', residu:0:15);
        WriteLn;
    
        // Test functions:
        f(x[1], f_check[1], deff);
        WriteLn('Check if the function values really are 0 at the found roots:');
        for i := 1 to n do
          WriteLn('Function #' + IntToStr(i) + ': ':20, f_check[i]:0:15);
      end else
        WriteLn('ERROR');
    
      ReadLn;
    end.
    

  


## Numerical integration of a function (unit _int_)

The NumLib function `int1fr` calculates the [integral](<https://en.wikipedia.org/wiki/Integral>) of a given function between limits `a` and `b` with a specified absolute accuracy `ae`: 
    
    
    procedure int1fr(f: rfunc1r; a, b, ae: ArbFloat; var integral, err: ArbFloat; var term: ArbInt);
    

  * `f` points to the function to be integrated. It must be a function of a single real variable (type `ArbFloat`) and return an `Arbfloat`. See the type `rfunc1r` declared in unit _typ_.
  * `a, b` are the limits of integration. `a` and/or `b` can attain the values `+/-Infinity` for integrating over an infinite interval. The order of `a` and `b` is handled in the mathematically correct sense.
  * `ae` determines the absolute accuracy requested.
  * `integreal` returns the value of the integral. It is only valid if `term = 1`.
  * `err` returns the achieved accuracy if the specified accuracy could not be reached. `term` has the value 2 in this case.
  * `term` returns an error code: 
    * 1 - successful termination, the integral could be calculated with absolute accuracy `ae`.
    * 2 - the requested accuracy could not be reached. But the integral is approximated within the accuracy `err`.
    * 3 - incorrect input data: `ae < 0`, or `a = b = infinity`, or `a = b = -infinity`
    * 4 - the integral could not be calculated: divergence, or too-slow convergence.



  
**Example**

Calculate the integral 

    [math]\displaystyle{ \int_a^b \frac 1 {x^2} \mathrm{d}x }[/math]

for several integration limits `a` and `b`. Since the function diverges at _x = 0_ the interval from _a_ to _b_ must not contain this point. The analytic result is [math]\displaystyle{ -1/{b} + 1/{a} }[/math]
    
    
    program integrate;
    
    uses
      SysUtils, typ, int;
    
    function recipx2(x: ArbFloat): ArbFloat;
    begin
      Result := 1.0 / sqr(x);
    end;
    
    function integral_recipx2(a, b: ArbFloat): Arbfloat;
    begin
      if a = 0 then
        a := Infinity
      else if a = Infinity then
        a := 0.0
      else
        a := -1/a;
      if b = 0 then
        b := Infinity
      else if b = Infinity then
        b := 0.0
      else
        b := -1/b;
      Result := b - a;
    end;
    
    procedure Execute(a, b: ArbFloat);
    var
      err: ArbFloat = 0.0;
      term: ArbInt = 0;
      integral: ArbFloat = 0.0;
    begin
      try
        int1fr(@recipx2, a, b, 1e-9, integral, err, term);
      except
        term := 4;
      end;
      Write('  The integral from ' + FloatToStr(a) + ' to ' + FloatToStr(b));
      case term of
        1: WriteLn(' is ', integral:0:9, ', exected: ', integral_recipx2(a, b):0:9);
        2: WriteLn(' is ', integral:0:9, ', error: ', err:0:9, ', exected: ', integral_recipx2(a, b):0:9);
        3: WriteLn(' cannot be calculated: Error in input data');
        4: WriteLn(' cannot be calculated: Divergent, or calculation converges too slowly.');
      end;
    end;
    
    begin
      WriteLn('Integral of f(x) = 1/x^2');
      Execute(1.0, 2.0);
      Execute(1.0, 1.0);
      Execute(2.0, 1.0);  
      Execute(1.0, Infinity);
      Execute(-Infinity, -1.0);
      Execute(0.0, Infinity);
      // NOTE: The next line will raise an exception if run in the IDE. This will not happen outside the IDE.
      // Integrate(-1.0, Infinity);
    end.
    

  


## Ordinary differential equations (unit _ode_)

### Solving a single first-order differential equation

The routine `odeiv1` solves an initial value problem described by a first-order differential equation of the form [math]\displaystyle{ \begin{cases} y' = f(x, y), \qquad x \in [a, b]\\\ y(a) = \alpha \end{cases} }[/math] with given _a_ , _b_ , _f_ and initial condition _y_(_a_) = α. 
    
    
    procedure odeiv1(f: rfunc2r; a, ya: ArbFloat; var b, yb: ArbFloat; ae: ArbFloat; var term: ArbInt);
    

  * `f` is the funtion to be processed. It depends on two real variables and returns a real value, as declared in unit _typ_ as `type rfunc2r = function(x, y: ArbFloat): ArbFloat`.
  * `a` is the _x_ value of the starting point of the interval in which the solution is determined.
  * `ya` is the value _α_ of the function at the starting point _a_ (initial value).
  * `b` is the _x_ value of the end point of the interval in which the solution is determined. The case `a > b` is allowed. If, after the calculation, the error code `term` is 2 then the interval has been changed, and `b` contains the new endpoint to which the solution could be calculated with the desired accuracy, `ae`. In all other cases, `b` is unchanged.
  * `yb` returns the result of the calculation if `term` < 3\. It is the value of the function at the end point _b_ of the interval. If `term = 3` then the result is undefined.
  * `ae` defines the absolute accuracy with which the value _y_(_b_) must be determined.
  * `term` returns an error code: 
    * 1 - successful completion
    * 2 - the solution could not reach the point _b_ with the required accuracy. However, `yb` is a sufficiently accurate approximation of y at the delivered `b`.
    * 3 - input error: `ae <= 0`



The algorithm is adaptive and is based on an [explicit one-step Runge-Kutta method](<https://en.wikipedia.org/wiki/Runge%E2%80%93Kutta_methods>) of order five. 

The procedure is not suitable for a rigid differential comparison. 

The accuracy can not be guaranteed in all cases. In so-called unstable problems, e.g. small variations in _y_(_a_) give large variations in _y_(_b_), the error may be significantly larger than what the result of the procedure suggests. Call the procedure with different values of `ae` to investigate this case. 

If one wants to solve the initial value problem for a number of points, e.g. from _x_ = 0 to _x_ = 1 with step size 0.1, then it is advisable to "integrate" and thus not to restart at _x_ = 0 with every new step. 

  
**Example**

Solve the differential equation _y"_ = -10 (y - _x_ 2) with the initial condition _y_(0) = 0 for _x_ = 0, 0.5, ..., 5. Compare with the exact solution _y_(_x_) = -0.02 exp(-10 _x_) + _x_ 2 \- 0.2 _x_ \+ 0.02 
    
    
    program solve_ode;
    
    uses
      typ, ode;
    
      function f(x, y: ArbFloat): Arbfloat;
      begin
        Result := -10 * (y - sqr(x));
      end;
    
      function exact(x: real): real; far;
      begin
        Result := -0.02*exp(-10*x) + sqr(x) - 0.2*x + 0.02;
      end;
    
    const
      d = 0.5;         // interval length
      ae = 1e-5;
      n = 10;
    
    var
      a, b: ArbFloat;
      ya, yb: ArbFloat;
      term: ArbInt;
      i: Arbint;
    
    begin
      // Set initial conditions
      a := 0.0;
      b := a + d;
      ya := 0.0;
    
      WriteLn('x':12, 'y':12, 'exact':12, 'error code':17);
      WriteLn;
      WriteLn(a:12:5, ya:12:5, exact(a):12:5, '-':17);
    
      for i := 1 to n do begin
        odeiv1(@f, a, ya, b, yb, ae, term);
        WriteLn(b:12:5, yb:12:5, exact(b):12:5, term:17);
        a := b;
        ya := yb;
        b := b + d;
      end;
    end.
    

  


### Solving a system of first-order differential equations

An initial value problem described by system of first-order differential equations can be solved by the procedure `odeiv2`: [math]\displaystyle{ \begin{cases} \mathbf{y}' = \mathbf{f}(x, \mathbf{y}), \qquad x \in [a, b]\\\ \mathbf{y}(a) = \mathbf{\alpha} \end{cases} }[/math]

where 

  * _**y**_ is an unknown n vector



    

    

    _y_ : [_a_ , _b_] --> **R** n
    or: _y_ = [_y 1_(_x_), _y 2_(_x_), ..., _y n_(_x_)]

  * _**f**_ is a vector function



    

    

    _**f**_ : [_a_ , _b_] --> **R** n
    or: _**f**_(_x_ , _y_) = [_f_ 1(_x_ ,_y_), f2(_x_ ,_y_), ..., fn(_x_ ,_y_)]

  * _a_ , _b_ , and the initial conditions _**α**_ = _**y**_(_a_) = [_y 1_(_a_), _y 2_(_a_), ... _y n_(_a_)] are given.



The algorithm is based on an [explicit one-step Runge-Kutta method](<https://en.wikipedia.org/wiki/Runge%E2%80%93Kutta_methods>) of order five with variable step size. 

  

    
    
    procedure odeiv2(f: oderk1n; a: ArbFloat; var ya, b, yb: ArbFloat; n: ArbInt; ae: ArbFloat; var term: ArbInt);
    

  * The parameter `f` contains the name of the procedure that calculates the function values _**f i**_(_x_ , _y i_). The declaration of the procedure must match the `type oderk1n = procedure(x: ArbFloat; var y, fxy: ArbFloat)` declared in unit _typ_. Here `x` is the coordinate in the interval [_a_ , _b_], and `y` is the first value of an array of _y_ values to be used in each equation of the ODE system. The function result is returned in another array which is specified by its first value `fxy`. Both arrays must be dimensioned to provide at least _n_ elements.
  * `a` is the begin of the calculation interval.
  * `ya` is the first element of an array containing the initial conditions for each ODE of the system, i.e. the function values _y_ i(_a_) = _α i_. The array must be allocated to containt at least _n_ elements.
  * `b` is the end of the calculation interval. The case `a > b` is allowed. If after the calculation `term` is 2 then the interval has been changed, and `b` contains the new endpoint to which the solution could have been calculated with the desired accuracy, `ae`. In all other cases, `b` is unchanged.
  * `yb` is the first element of an array which receives the results of the calculation. It must be dimensioned to contain at least n elements.
  * `n` denotes the number of equations in the system.
  * `ae` specifies the absolute accuracy with which the solution must be calculated.
  * `term` is an error code: 
    * 1 - successful completion.
    * 2 - the solution could not reach the point _b_ with the required accuracy. However, the values in the `yb` array are sufficiently accurate approximations of the _y_ values at the delivered `b`.
    * 3 - input error: `n < 1`, or `ae <= 0`.



  
**Example**

Integrate the following initial value problem between _x_ = 0 and _x_ = 1 with step size 0.1: [math]\displaystyle{ \begin{cases} y'_{1}=2xy_{1}+y_{2} \\\ y'_{2}=-y_{1}+2xy_{2}\\\ y_1(0)=0,\ y_2(0)=1 \end{cases} }[/math]

The exact solutions are [math]\displaystyle{ \begin{cases} y_1(x)=\exp({x^2}) \sin(x) \\\ y_2(x)=\exp({x^2}) \cos(x) \end{cases} }[/math]
    
    
    program solve_ode_sys;
    
    uses
      typ, ode;
    
    const
      ae = 1e-5;
    
      procedure f(x: ArbFloat; var y, fxy: ArbFloat);
      var
        _y: array[1..2] of ArbFloat absolute y;
        _fxy: array[1..2] of ArbFloat absolute fxy;
      begin
        _fxy[1] := 2*x*_y[1] + _y[2];
        _fxy[2] := -_y[1] + 2*x*_y[2]
      end;
    
      function exact1(x: ArbFloat): ArbFloat;
      begin
        Result := exp(x*x) * sin(x);
      end;
    
      function exact2(x: ArbFloat): ArbFloat;
      begin
        Result := exp(x*x) * cos(x);
      end;
    
    var
      a, b, d: ArbFloat;
      ya: array[1..2] of ArbFloat;
      yb: array[1..2] of ArbFloat;
      term: ArbInt;
      i: Arbint;
      n: ArbInt;
    
    begin
      // Set initial conditions
      a := 0.0;
      b := 0.1;
      d := b - a;
      ya[1] := 0.0;
      ya[2] := 1.0;
      n := 10;
    
      WriteLn('x':12, 'y[1]':12, 'y[2]':12, 'exact[1]':12, 'exact[2]':12, 'error code':17);
      WriteLn;
      WriteLn(a:12:5, ya[1]:12:5, ya[2]:12:5, exact1(a):12:5, exact2(a):12:5, '-':17);
    
      for i := 1 to n do begin
        odeiv2(@f, a, ya[1], b, yb[1], 2, ae, term);
        WriteLn(b:12:5, yb[1]:12:5, yb[2]:12:5, exact1(b):12:5, exact2(b):12:5, term:17);
        a := b;
        ya[1] := yb[1];
        ya[2] := yb[2];
        b := b + d;
      end;
    end.
    

  


## Interpolation and fitting (unit _ipf_)

Unit _ipf_ contains routines for 

  * fitting a set of data points with a polynomial,
  * interpolating or fitting a set of data points with a natural cubic spline. A [spline](<https://en.wikipedia.org/wiki/Spline_\(mathematics\)>) is a piecewise defined function of polynomials which has a high degree of smoothness at the connection points of the polynomials ("knots"). In case of a _natural cubic_ spline, the polynomials have degree 3 and their second derivative is zero at the first and last knot.



Fitting means: determine an approximating smooth function such that the deviations from the data points are at minimum.  
Interpolation means: determine a function that runs exactly through the data points. 

Use of polynomials or splines is recommended unless the data are known to belong to a known function in which there are still some unknown parameters, for example, the data are measurements of the function _f(x) = a + b e -c x._

In the most common case, the data contain measurement errors and appear to follow the shape of a "simple" function. Therefore, it is recommended to first try to fit with a low-degree polynomial, e.g. not higher than degree 5. If this is not successful or if the shape of the function is more complicated try to fit with a spline. 

If the data contain no (or very little) measurement noise, it is almost always wise to interpolate the data with a spline. 

  


### Fitting with a polynomial

For this purpose, the routine `ipfpol` can be applied. It uses given data points (_x i_, _y i_), _i_ = 1, ..., _m_ , and _n_ given coefficients _b 0_, ..., _b n_ to calculate a polynomial [math]\displaystyle{ \operatorname{p}(x) = b_0 + b_1 x + \dots + b_n x^n }[/math]

In the sense of the least squares fitting, the coefficients _b 0_ ... _b n_ are adjusted to minimize the expression 

[math]\displaystyle{ \sum_{i=1}^m {(\operatorname{p}(x) - y_i)^2} }[/math]
    
    
    procedure ipfpol(m, n: ArbInt; var x, y, b: ArbFloat; var term: ArbInt);
    

  * `m` denotes the number of data points. It is required that `m ≥ 1 `
  * `n` is the degree of the polynomial to be fitted. It is required that `n > 0`.
  * `x` is the first element of a one-dimensional array containing the _x_ values of the data points. The array must be allocated to contain at least `m` values.
  * `y` is the first element of a one-dimensional array containing the _y_ values of the data points. The array must be dimensioned to contain at least `m` values.
  * `b` is the first element of a one-dimenisonal array in which the determined polynomial coefficients are returned if `term` is 1. The array must have been dimensioned to provide space for at least `n + 1` values. The returned coefficients are ordered from lowest to highest degree.
  * `term` returns an error code: 
    * 1 - successful completion, the array `b` contains valid coefficients
    * 3 - input error: `n < 0`, or `m < 1`.



After the coefficients _b 0_, ..., _b n_ once have been determined the value of p(_x_) can be calculated for any _x_ using the procedure `spepol` from the unit _spe_. 

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** With this routine, of course, an interpolating polynomial can also be determined by equaling _n_ to _m_ \- 1.

  
**Example**

Fit a polynomial of degree 2 to these data points: [math]\displaystyle{ \begin{array}{ccc} \hline i & x_i & y_i \\\ \hline 1 & 0.00 & 1.26 \\\ 2 & 0.08 & 1.37 \\\ 3 & 0.22 & 1.72 \\\ 4 & 0.33 & 2.08 \\\ 5 & 0.46 & 2.31 \\\ 6 & 0.52 & 2.64 \\\ 7 & 0.67 & 3.12 \\\ 8 & 0.81 & 3.48 \end{array} }[/math]

[![numlib ipfpol.png](https://wiki.freepascal.org/images/a/a6/numlib_ipfpol.png)](</File:numlib_ipfpol.png>)
    
    
    program polynomial_fit;
    
    uses
      SysUtils, StrUtils, typ, ipf, spe;
    
    const
      m = 8;
      n = 2;
      x: array[1..m] of Arbfloat = (
        0.00, 0.08, 0.22, 0.33, 0.46, 0.52, 0.67, 0.81
      );
      y: array[1..m] of Arbfloat = (
        1.26, 1.37, 1.72, 2.08, 2.31, 2.64, 3.12, 3.48
      );
    
    var
      b: array[0..n] of ArbFloat;
      i: Integer;
      term: ArbInt;
      xint, yint: ArbFloat;
    begin
      WriteLn('Fitting a polynomial of degree 2: p(x) = b[0] + b[1] x + b[2] x^2');
      WriteLn;
    
      WriteLn('Data points');
      WriteLn('i':10, 'xi':10, 'yi':10);
      for i:=1 to m do
        WriteLn(i:10, x[i]:10:2, y[i]:10:2);
      WriteLn;
    
      // Execute fit
      ipfpol(m, n, x[1], y[1], b[0], term);
    
      // Display fitted polynomial coefficients
      WriteLn('Fitted coefficients b = ');
      WriteLn(b[0]:10:6, b[1]:10:6, b[2]:10:6);
      WriteLn;
    
      // Interpolate and display fitted polynomial for some x values
      WriteLn('Some interpolated data');
      WriteLn('':10, 'x':10, 'y':10);
      for i:= 1 to 5 do begin
        xint := 0.2*i;
        yint := spepol(xint, b[0], n);
        WriteLn('':10, xint:10:2, yint:10:2);
      end;
    end.
    

  


### Interpolation with a natural cubic spline

The routine `ipfisn` can be used for this purpose. It calculates the parameters of the spline. Once these parameters are known, the value of the spline can be calculated at any point using the `ipfspn` procedure. 

Assume that (_x i_, yi _),_ i _= 0, ...,_ n' are the given data points with _x 0_ < _x 1_ < ... < _x n_. Then assume a function _g(x)_ which is twice continuously differentiable (i.e., the first and second derivatives exist and are continuous) and which interpolates the data (i.e., _g(x i = yi_). Imagine _g(x)_ as the shape of a thin flexible bar. Then the bending energy (in first approximation) is proportional to 

[math]\displaystyle{ \int_{-\infty}^{\infty} g''(x)^2 dx }[/math]

The interpolating natural spline _s_ can be understood as the function _g_ which minimizes this integral and therefore the bending energy. 

The spline _s_ has the following properties: 

  * _s_ is a cubic polynomial on the interval [_x i_, _x i+1_]
  * it is twice continuously differentiable on (-∞, ∞)
  * _s"(x 0)_ = _s"(x n)_ = 0
  * on (-∞, _x 0_) and (_x n, ∞)_ _s_ is linear.



  

    
    
    procedure ipfisn(n: ArbInt; var x, y, d2s: ArbFloat; var term: ArbInt);
    

  * Calculates the second derivatives of the splines at the knots _x i_.
  * `n` is the index of the last data point, i.e. there are `n+1` data points.
  * `x` is the first element of the array with the x coordinates of the data points. It must be dimensioned to contain at last `n+1` values. The standard declaration would be `var x: array[0..n] of ArbFloat`. The data points must be in strictly ascending order, i.e. there must not be any data points for which _x i_ >= _x i+1_.
  * `y` is the first element of the array with the y coordinates of the data points. It must be dimensioned to contain at last `n+1` values. The standard declaration would be `var y: array[0..n] of ArbFloat`.
  * `d2s` is the first element of the array which receives the second derivatives of the splines at the knots if the returned error code `term` is 1. Note that the array does not contain values for the first and last data point because they are automatically set to 0 (_natural_ splines). Therefore the array must be dimensioned to contain at least `n-1` elements. The standard declaration would be: `var d2s: array[1..n-1] of ArbFloat`
  * `term` returns an error code: 
    * 1 - successful completion, the array beginning at `d2s` contains valid data
    * 2 - calculation of the second derivatives was not successful
    * 3 - incorrect input parameters: _n_ ≤ 1, or _x i_ ≥ _x i+1_



  

    
    
    function  ipfspn(n: ArbInt; var x, y, d2s: ArbFloat; t: ArbFloat; var term: ArbInt): ArbFloat;
    

  * Calculates the value of the spline function at any coordinate _t_. The calculation uses the second derivatives determined by `ipnisn`.
  * `n`, `x`, `y`, `d2s` have the same meaning as above
  * `term` returns an error code: 
    * 1 - successful completion
    * 3 - The calculation could not be executed because _n_ ≤ 1.



  
**Example**

Interpolate a natural cubic spline through the following data points: [math]\displaystyle{ \begin{array}{ccc} \hline i & x_i & y_i \\\ \hline 0 & 0.00 & 0.980 \\\ 1 & 0.09 & 0.927 \\\ 2 & 0.22 & 0.732 \\\ 3 & 0.34 & 0.542 \\\ 4 & 0.47 & 0.385 \\\ 5 & 0.58 & 0.292 \\\ 6 & 0.65 & 0.248 \\\ 7 & 0.80 & 0.179 \\\ 8 & 0.93 & 0.139 \end{array} }[/math]

[![numlib ipfisn.png](https://wiki.freepascal.org/images/2/2c/numlib_ipfisn.png)](</File:numlib_ipfisn.png>)
    
    
    program spline_interpolation;
    
    uses
      typ, ipf;
    
    const
      n = 8;
      x: array[0..n] of Arbfloat = (
        0.00, 0.09, 0.22, 0.34, 0.47, 0.58, 0.65, 0.80, 0.93
      );
      y: array[0..n] of Arbfloat = (
        0.990, 0.927, 0.734, 0.542, 0.388, 0.292, 0.248, 0.179, 0.139
      );
    
    var
      s: array[0..n] of ArbFloat;
      d2s: array[1..n-1] of ArbFloat;
      i: Integer;
      term: ArbInt;
      xint, yint: ArbFloat;
    begin
      WriteLn('Interpolation with a natural cubic spline');
      WriteLn;
    
      WriteLn('Data points');
      WriteLn('i':10, 'xi':10, 'yi':10);
      for i:=0 to n do
        WriteLn(i:10, x[i]:10:2, y[i]:10:3);
      WriteLn;
    
      // Interpolation
      ipfisn(n, x[0], y[0], d2s[1], term);
    
      // Display 2nd derivatives of splines at xi
      WriteLn('s"(xi)');
      Write(0.0:10:6);      // 2nd derivative of 1st point is 0
      for i:=1 to n-1 do
        Write(d2s[i]:10:6);
      WriteLn(0.0:10:6);    // 2nd derivative of last point is 0
      WriteLn;
    
      // Interpolate for some x values
      WriteLn('Some interpolated data');
      WriteLn('':10, 'x':10, 'y':10);
      for i:= 1 to 5 do begin
        xint := 0.2 * i;
        yint := ipfspn(n, x[0], y[0], d2s[1], xint, term);
        WriteLn('':10, xint:10:2, yint:10:2);
      end;
    
    end.
    

  


### Fitting with a natural cubic spline

The routine `ipffsn` can be used for this purpose. This routine calculates the spline-fit parameters. Once these parameters are known the value of the spline can be calculated at any point using the procedure `ipfspn`. 

Following the model with the bending energy in the previous chapter the calculated fit minimizes 

[math]\displaystyle{ \int_{-\infty}^{\infty} g''(x)^2 dx + \lambda \sum_{i=0}^n (g(x_i) - y_i)^2 }[/math]

where the parameter λ must be determined. It is a measure of the compromise between the "smoothness" of the fit - which is measured by the integral (the bending energy) - and the deviation of the fit to the given data - which is measured by the sum of squares. The solution is again a natural cubic spline. 

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** The procedure `ipffsn` is buggy and crashes. Therefore, it is not documented at the moment. 

  


## Special functions (unit _spe_)

### Evaluation of a polynomial

[Horner's scheme](<https://en.wikipedia.org/wiki/Horner%27s_method>) provides an efficient method for evaluating a [polynomial](<https://en.wikipedia.org/wiki/Polynomial>) at a specific x value: 

    [math]\displaystyle{ \operatorname{p}(x) = a_0 + a_1 x + a_2 x^2 + ... + a_n x^n \ \Rightarrow \ \operatorname{p}(x) = a_0 + x (a_1 + x (a_2 + \dots + x (a_{n-1} + x a_n))) }[/math]

This method is applied in NumLib's procedure `spepol`: 
    
    
    function spepol(x: ArbFloat; var a: ArbFloat; n: ArbInt): ArbFloat;
    

  * `x` is the argument at which the polynomial is evaluated.
  * `a` is the first element of an array containing the polynomial coefficients a0, a1, a2, ... an. The coefficient must be ordered from lowest to highest degree.
  * `n` is the degree of the polynomial, i.e. there are `n+1` elements in the array.



  
**Example**

Evaluate the polynomial 

    [math]\displaystyle{ \operatorname{p}(x) = 2 x^3 - 6 x^2 + 2 x - 1 }[/math]

at x = 3. Note that the terms are not in the correct order for `spepol` and rearrange them to 

    [math]\displaystyle{ \operatorname{p}(x) = -1 + 2 x - 6 x^2 + 2 x^3 }[/math]
    
    
    program polynomial;
    
    uses
      typ, spe;
    
    const
      n = 3;
      a: array[0..n] of ArbFloat = (-1, 2, -6, 2);
    
    var
      x, y: ArbFloat;
    begin
      x := 3.0;
      y := spepol(x, a[0], n);
      WriteLn(y:0:6);
    end.
    

  


### Error function

[![numlib erf erfc.png](https://wiki.freepascal.org/images/b/b8/numlib_erf_erfc.png)](</File:numlib_erf_erfc.png>)

The [error function _erf(x)_](<https://en.wikipedia.org/wiki/Error_function>) and its complement, the [complementary error function _erfc(x)_](<https://en.wikipedia.org/wiki/Error_function#Complementary_error_function>), are the lower and upper integrals of the Gaussian function, normalized to unity: 

    [math]\displaystyle{ \operatorname{erf}(x) = \frac {2} {\sqrt{\pi}} \int_0^x e^{-{t}^2} dt }[/math]

    [math]\displaystyle{ \operatorname{erfc}(x) = \frac {2} {\sqrt{\pi}} \int_x^\infty e^{-{t}^2} dt = {1} - \operatorname{erf}(x) }[/math]

Both functions can be calculated by the NumLib functions `speerf` and `speefc`, respectively 
    
    
    function speerf(x: ArbFloat): ArbFloat;   // --> erf(x)
    function speefc(x: ArbFloat): ArbFloat;   // --> erfc(x)
    

  
**Example**

A table of some values of the error function and the complementary error function is calculated in the following demo project: 
    
    
    program erf_Table;
    
    uses
      SysUtils, StrUtils, typ, spe;
    
    const
      Wx = 7;
      Wy = 20;
      D = 6;
    
    var
      i: Integer;
      x: ArbFloat;
      fs: TFormatSettings;
    begin
      fs := DefaultFormatSettings;
      fs.DecimalSeparator := '.';
      WriteLn('x':Wx, 'erf(x)':Wy+1, 'erfc(x)':Wy+1);
      WriteLn;
      for i:= 0 to 20 do begin
        x := 0.1*i;
        WriteLn(Format('%*.1f %*.*f %*.*f', [
          Wx, x,
          Wy, D, speerf(x),
          Wy, D, speefc(x)
        ], fs));
      end;
    end.
    

  


### Normal distribution

[![numlib normaldist.png](https://wiki.freepascal.org/images/c/c2/numlib_normaldist.png)](</File:numlib_normaldist.png>)

The (cumulative) [normal distribution](<https://en.wikipedia.org/wiki/Normal_distribution>) is the integral of the Gaussian function with zero mean and unit standard deviation, i.e. 

[math]\displaystyle{ \operatorname{N}(x) = \frac{1}{\sqrt{2\pi}} \int_{-\infty}^{x} \operatorname{exp}(\frac{t^2}{2}) dt = \frac{1}{2}[1 + \operatorname{erf} (\frac{x}{\sqrt{2}})] }[/math]

Suppose a set of random floating point numbers which are distributed according to this formula. Then _N(x)_ measures the probability that a number from this set is smaller than _x_. 

In NumLib, the **normal distribution** is calculated by the function `normaldist`: 
    
    
    function normaldist(x: ArbFloat): ArbFloat;
    

  * `x` can take any value between -∞ and +∞.
  * The function result is between 0 and 1.



  
The **inverse normal distribution** of y returns the x value for which the normal distribution _N(x)_ has the value y. 
    
    
    function invnormaldist(y: ArbFloat): ArbFloat;
    

  * `y` is allowed only between 0 and 1.
  * The function can take any value between -∞ and +∞.



[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** The functions `normaldist` and `invnormaldist` are not available in fpc 3.0.x and older.

  
**Example**

Calculate the probabilities for _x_ = -10, -8, ..., 12 for a normal distribution with mean _μ_ = 2 and standard deviation _σ_ = 5. In order to reduce this distribution to the standard normal distribution implemented by NumLib, we introduce a transformation _x r_ = (_x_ \- _μ_)/_σ_. 
    
    
    program normdist;
    
    uses
      typ, spe;
    
    const
      mu = 2.0;
      sigma = 5.0;
      x: array[0..12] of ArbFloat = (-10, -8, -6, -4, -2, 0, 2, 4, 6, 8, 10, 12, 14);
    
    var
      xr, y: ArbFloat;
      i: Integer;
    begin
      WriteLn('Normal distribution with mean ', mu:0:1, ' and standard deviation ', sigma:0:1);
      WriteLn;
      WriteLn('x':20, 'y':20);
    
      for i:= 0 to High(x) do begin
        xr := (x[i] - mu) / sigma;
        y := normaldist(xr);
        WriteLn(x[i]:20:1, y:20:6);
      end;
    end.
    

  


### Gamma function

[![numlib gamma.png](https://wiki.freepascal.org/images/2/20/numlib_gamma.png)](</File:numlib_gamma.png>)

The [gamma function](<https://en.wikipedia.org/wiki/Gamma_function>) is needed by many probability functions. It is defined by the integral 

    [math]\displaystyle{ \Gamma({x}) = \int_0^{\infty}t^{x-1} e^{-t} dt }[/math]

NumLib provides two functions for its calculation: 
    
    
    function spegam(x: ArbFloat): ArbFloat;
    function spelga(x: ArbFloat): ArbFloat;
    

The first one, `spegam`, calculates the function directly. But since the gamma function grows rapidly for even not-too large arguments this calculation very easily overflows. 

The second function, `spelga` calculates the natural logarithm of the gamma function which is more suitable to combinatorial calculations where multiplying and dividing the large gamma values can be avoided by adding or subtracting their logarithms. 

  
**Example**

The following demo project prints a table of some values of the gamma function and of its logarithm. Note that `spegam(x)` overflows above about _x = 170_ for data type `extended`. 
    
    
    program Gamma_Table;
    
    uses
      SysUtils, StrUtils, typ, spe;
    
    const
      VALUES: array[0..2] of ArbFloat = (1, 2, 5);
      Wx = 7;
      Wy = 30;
      Wln = 20;
    
    var
      i: Integer;
      x: ArbFloat;
      magnitude: ArbFloat;
    begin
      WriteLn('x':Wx, 'Gamma(x)':Wy, 'ln(Gamma(x))':Wln);
      WriteLn;
      magnitude := 1E-3;
      while magnitude <= 1000 do begin
        for i := 0 to High(VALUES) do begin
          x := VALUES[i] * magnitude;
          Write(PadLeft(FloatToStr(x), Wx));
          if x <= 170 then  // Extended overflow above 170
            Write(FormatFloat('0.000', spegam(x)):Wy)
          else
            Write('overflow':Wy);
          WriteLn(spelga(x):Wln:3);
          if abs(x-1000) < 1E-6 then
            break;
        end;
        magnitude := magnitude * 10;
      end;
    end.
    

  


### Incomplete gamma function

[![numlib gammapq.png](https://wiki.freepascal.org/images/d/d0/numlib_gammapq.png)](</File:numlib_gammapq.png>)

The lower and upper [incomplete gamma functions](<https://en.wikipedia.org/wiki/Incomplete_Gamma_function>) are defined by 

    [math]\displaystyle{ \operatorname{P}({s},{x}) = \frac{1}{\Gamma({s})} \int_0^{x}t^{s-1} e^{-t} dt }[/math]

    [math]\displaystyle{ \operatorname{Q}({s},{x}) = \frac{1}{\Gamma({s})} \int_{x}^{\infty}t^{s-1} e^{-t} dt = 1 - \operatorname{P}({s}, {x}) }[/math]
    
    
    function gammap(s, x: ArbFloat): ArbFloat;
    function gammaq(s, x: ArbFloat): ArbFloat;
    

  * The parameter `s` must be positive: _s_ > 0
  * The argument `x` must not be negative: _x_ ≥ 0
  * The function results are in the interval [0, 1].



[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** The functions `gammap` and `gammaq` are not available in fpc v3.0.x and older.

  
**Example**

The following project prints out a table of `gammap` and `gammaq` values: 
    
    
    program IncompleteGamma_Table;
    
    uses
      SysUtils, StrUtils, typ, spe;
    
    const
      s = 1.25;
    
      Wx = 7;
      Wy = 20;
      D = 6;
    
    var
      i: Integer;
      x: ArbFloat;
      fs: TFormatSettings;
    begin
      fs := DefaultFormatSettings;
      fs.DecimalSeparator := '.';
      WriteLn('x':Wx, 'GammaP('+FloatToStr(s,fs)+', x)':Wy+1, 'GammaQ('+FloatToStr(s,fs)+', x)':Wy+1);
      WriteLn;
      for i := 0 to 20 do begin
        x := 0.5*i;
        WriteLn(Format('%*.1f %*.*f %*.*f', [
          Wx, x,
          Wy, D, gammap(s, x),
          Wy, D, gammaq(s, x)
        ], fs));
      end;
    end.
    

  


### Beta function

[![numlib beta.png](https://wiki.freepascal.org/images/a/aa/numlib_beta.png)](</File:numlib_beta.png>)

The [Beta function](<https://en.wikipedia.org/wiki/Beta_function>) is another basis of important functions used in statistics. It is defined as 

    [math]\displaystyle{ \operatorname{B}(a, b) = \frac{{\Gamma(a)}{\Gamma(b)}}{\Gamma(a+b)} = \int_0^1{t^{x-1} (1-t)^{y-1} dt} }[/math]

The NumLib routine for its calculation is 
    
    
    function beta(a, b: ArbFloat): ArbFloat;
    

  * The arguments `a` and `b` must be positive: `_a_ > 0` and `_b_ > 1`.
  * The calculation is based on the logarithms of the gamma function and therefore does not suffer from the overflow issues of the gamma function.



[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** The function `beta` is not available in fpc v3.0.x or older.

  


### Incomplete beta function

[![numlib betai.png](https://wiki.freepascal.org/images/6/61/numlib_betai.png)](</File:numlib_betai.png>)

The (regularized) [incomplete beta function](<https://en.wikipedia.org/wiki/Beta_function#Incomplete_beta_function>) is defined by 

    [math]\displaystyle{ \operatorname{I}_x(a,b) = \frac {1}{\operatorname{B}(ab)} \int_0^x{t^{x-1} (1-t)^{y-1} dt} }[/math]
    
    
    function betai(a, b, x: ArbFloat): ArbFloat;
    

  * The arguments `a` and `b` must be positive: `_a_ > 0` and `_b_ > 1`.
  * `x` is only allowed to be in the range 0 ≤ _x_ ≤ 1.
  * The function result is in the interval [0, 1].



The inverse incomplete beta function determines the argument x for which the passed parameter is equal to `_I x(a, b)_`: 
    
    
    function invbetai(a, b, y: ArbFloat): ArbFloat;
    

  * The arguments `a` and `b` must be positive: `_a_ > 0` and `_b_ > 1`.
  * `y` is only allowed to be in the range 0 ≤ _x_ ≤ 1.
  * The function result is in the interval [0, 1].



[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** The functions `betai` and `invbetai` are not available in fpc v3.0.x and older.

  
**Example**

Calculate the incomplete beta function for some values of x, a, b, and apply the result to the inverse incomplete beta function to demonstrate the validity of the calculations. 
    
    
    program incomplete_beta_func;
    
    uses
      SysUtils, typ, spe;
    
      procedure TestBetai(a, b, x: ArbFloat);
      var
        xc, y: ArbFloat;
      begin
        y := betai(a, b, x);
        xc := invbetai(a, b, y, macheps);
        WriteLn('  x = ', x:0:6, '  --->  y = ', y:0:9, '  --->  x = ', xc:0:6);
      end;
    
    begin
      WriteLn('Incomplete beta function and its inverse');
      WriteLn;
      WriteLn('        y = betai(1, 3, x)      x = invbetai(1, 3, y)');
      TestBetaI(1, 3, 0.001);
      TestBetaI(1, 3, 0.01);
      TestBetaI(1, 3, 0.1);
      TestBetaI(1, 3, 0.2);
      TestBetaI(1, 3, 0.9);
      TestBetaI(1, 3, 0.99);
      TestBetaI(1, 3, 0.999);
      WriteLn;
    
      WriteLn('        y = betai(1, 1, x)      x = invbetai(1, 1, y)');
      TestBetaI(1, 1, 0.001);
      TestBetaI(1, 1, 0.01);
      TestBetaI(1, 1, 0.1);
      TestBetaI(1, 1, 0.2);
      TestBetaI(1, 1, 0.9);
      TestBetaI(1, 1, 0.99);
      TestBetaI(1, 1, 0.999);
      WriteLn;
    
      WriteLn('        y = betai(3, 1, x)      x = invbetai(3, 1, y)');
      TestBetaI(3, 1, 0.001);
      TestBetaI(3, 1, 0.01);
      TestBetaI(3, 1, 0.1);
      TestBetaI(3, 1, 0.2);
      TestBetaI(3, 1, 0.9);
      TestBetaI(3, 1, 0.99);
      TestBetaI(3, 1, 0.999);
      WriteLn;
    end.
    

### Bessel functions

The [Bessel functions](<https://en.wikipedia.org/wiki/Bessel_function>) are solutions of the Bessel differential equation: 

    [math]\displaystyle{ {x}^2 y'' + x y' + ({x}^2 - \alpha^2) {y} = 0 }[/math]

The **Bessel functions of the first kind** , or **cylindrical harmonics** , _J α(x)_, are the solutions which are not singular at the origin, in contrast to the **Bessel functions of the second kind** , _Y α(x)_. _Y α(x)_ is only defined for x > 0\. 

**Example**

  * Calculate the probability that random normal-distributed numbers are within 1, 2, and 3 standard deviations around the mean.
  * Calculate the maximum deviation of normal-distributed deviates (in units of standard deviations) from the mean at probabilities 0.90, 0.95, 0.98, 0.99 and 0.999.



[![numlib bessel 1st.png](https://wiki.freepascal.org/images/b/ba/numlib_bessel_1st.png)](</File:numlib_bessel_1st.png>) [![numlib bessel 2nd.png](https://wiki.freepascal.org/images/f/f8/numlib_bessel_2nd.png)](</File:numlib_bessel_2nd.png>)

The solutions for a purely imaginary argument are called **modified Bessel functions of the first kind** , _I α(x)_, and **modified Bessel function of the second kind** , _K α(x)_. _K α(x)_ is only defined for x > 0\. 

[![numlib mod bessel 1st.png](https://wiki.freepascal.org/images/c/c6/numlib_mod_bessel_1st.png)](</File:numlib_mod_bessel_1st.png>) [![numlib mod bessel 2nd.png](https://wiki.freepascal.org/images/5/54/numlib_mod_bessel_2nd.png)](</File:numlib_mod_bessel_2nd.png>)

NumLib implements only the solutions for the parameters α = 0 and α = 1: 
    
    
    function spebj0(x: ArbFloat): ArbFloat;     // Bessel function of the first kind, J0 (alpha = 0)
    function spebj1(x: ArbFloat): ArbFloat;     // Bessel function of the first kind, J1 (alpha = 1)
    
    function speby0(x: ArbFloat): ArbFloat;     // Bessel function of the second kind, Y0 (alpha = 0)
    function speby1(x: ArbFloat): ArbFloat;     // Bessel function of the second kind, Y1 (alpha = 1)
    
    function spebi0(x: ArbFloat): ArbFloat;     // modified Bessel function of the first kind, I0 (alpha = 0)
    function spebi1(x: ArbFloat): ArbFloat;     // modified Bessel function of the first kind, I1 (alpha = 1)
    
    function spebk0(x: ArbFloat): ArbFloat;     // modified Bessel function of the second kind, K0 (alpha = 0)
    function spebk1(x: ArbFloat): ArbFloat;     // modified Bessel function of the second kind, K1 (alpha = 1)
    

  


### Other functions

The unit _spe_ contains some other functions which will not be documented here because they are already available in th FPC's standard unit _math_. 

Function  |  Equivalent function   
in _math_ | Description   
---|---|---  
`**function** speent(x: ArbFloat): LongInt;` | `floor(x)` | Entier function, calculates first integer smaller than or equal to `x`  
`**function** spemax(a, b: Arbfloat): ArbFloat;` | `max(a, b)` | Maximum of two floating point values   
`**function** spepow(a, b: ArbFloat): ArbFloat;` | `power(a, b)` | Calculates _a b_  
`**function** spesgn(x: ArbFloat): ArbInt;` | `sign(x)` | Returns the sign of `x` (`-1` for `x < 0`, `0` for `x = 0`, `+1` for `x > 0`)   
`**function** spears(x: ArbFloat): ArbFloat;` | `arcsin(x)` | Inverse function of `sin(x)`  
`**function** spearc(x: ArbFloat): ArbFloat;` | `arccos(x)` | Inverse function of `cos(x)`  
`**function** spesih(x: ArbFloat): ArbFloat;` | `sinh(x)` | Hyperbolic sine   
`**function** specoh(x: ArbFloat): ArbFloat;` | `cosh(x)` | Hyperbolic cosine   
`**function** spetah(x: ArbFloat): ArbFloat;` | `tanh(x)` | Hyperbolic tangent   
`**function** speash(x: ArbFloat): ArbFloat;` | `arcsinh(x)` | Inverse of the hyperbolic sine   
`**function** speach(x: ArbFloat): ArbFloat;` | `arccosh(x)` | Inverse of the hyperbolic cosine   
`**function** speath(x: ArbFloat): ArbFloat;` | `arctanh(x)` | Inverse of the hyperbolic tangent

---

_Source: [https://wiki.freepascal.org/NumLib_Documentation](https://web.archive.org/web/20240304010939/https://wiki.freepascal.org/NumLib_Documentation)_
