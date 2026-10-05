# Generating Random Numbers

│ **English (en)** │  **[русский (ru)](<../ru/Generating_Random_Numbers.md>)** │


[![fpc source logo.png](https://wiki.freepascal.org/images/e/e1/fpc_source_logo.png)](</File:fpc_source_logo.png>)

**Random numbers** are important resources for scientific applications, education, game development and visualization. They play a key role in numeric simulation. 

Algorithm-generated random numbers are pseudo-random numbers. They belong to a (large) set of repeating numbers, whose sequence is impossible or at least difficult to predict. Unlike Delphi, that uses a linear congruential generator.(See [Delphi compatible LCG Random](<Delphi_compatible_LCG_Random.md> "Delphi compatible LCG Random")), Free Pascal uses a MersenneTwister algorithm for its standard `[random](<http://lazarus-ccr.sourceforge.net/docs/rtl/system/random.html> "doc:rtl/system/random.html")` function as defined in [RTL](<RTL.md> "RTL"). Before its first use, FPC's random number generator has to be initialized with a single call of the `[randomize](<http://lazarus-ccr.sourceforge.net/docs/rtl/system/randomize.html> "doc:rtl/system/randomize.html")` function, which sets the seed of the generator. Preferably this is done in the launch phase of the program. 

Alternatively, on Unix- and Linux-based systems, the virtual devices `[/dev/random](<Dev_random.md> "Dev random")` and `/dev/urandom` are available. They generate (pseudo) random numbers based on hardware. 

A third option is to use random numbers from external sources, either from specialised hardware devices or from public sources, e.g. based on radioactive decay data. 

## Contents

  * 1 Uniform Distribution
  * 2 Normal (Gaussian) Distribution
  * 3 Exponential Distribution
  * 4 Gamma Distribution
  * 5 Erlang Distribution
  * 6 Poisson Distribution
  * 7 t Distribution
  * 8 Chi Squared Distribution
  * 9 F Distribution
  * 10 See also
  * 11 References



## Uniform Distribution

The continuous uniform distribution (also referred to as rectangular distribution) represents a family of symmetric probability distributions. Here, for each member of the family all intervals of the same length on the distribution's support are equally probable. 

The standard [RTL](<RTL.md> "RTL") function `[random](<http://lazarus-ccr.sourceforge.net/docs/rtl/system/random.html> "doc:rtl/system/random.html")` generates random numbers that fulfill a uniform distribution. If called without parameter `random` delivers a floating point pseudorandom number in the interval [0, 1), i.e. 0 <= result < 1\. if `random` is called with a longint argument L it delivers a longint random in the interval [0, L). 

A further set of uniformly distributed random number generators is presented in `[Marsaglia's pseudo random number generators.](<Marsaglia's_pseudo_random_number_generators.md> "Marsaglia's pseudo random number generators")`  
  
Uniformly distributed random numbers are not useful for every application. In order to create random numbers of other distributions special algorithms are necessary: 

## Normal (Gaussian) Distribution

One of the more common algorithms to produce normally distributed random numbers from uniformly distributed random numbers is the [Box-Müller approach](<http://en.wikipedia.org/wiki/Box–Muller_transform>). The following function calculates Gaussian-distributed random numbers: 
    
    
     function rnorm (mean, sd: real): real;
     {Calculates Gaussian random numbers according to the Box-Müller approach}
      var
       u1, u2: real;
     begin
       u1 := random;
       u2 := random;
       rnorm := mean * abs(1 + sqrt(-2 * (ln(u1))) * cos(2 * pi * u2) * sd);
      end;

The same algorithm is used by the [randg](<http://lazarus-ccr.sourceforge.net/docs/rtl/math/randg.html> "doc:rtl/math/randg.html") randg function from the [RTL](<RTL.md> "RTL") [math](<http://lazarus-ccr.sourceforge.net/docs/rtl/math/index.html> "doc:rtl/math/index.html") unit: 
    
    
    function randg(mean,stddev: float): float;

## Exponential Distribution

An exponential distribution occurs frequently in real-world problems. A classical example is the distribution of waiting times between independent Poisson-random events, e.g. the radioactive decay of nuclei [Press et al. 1989]. 

The following function delivers a single real random number out of an exponential distribution. _Rate_ is the inverse of the mean, and the constant _RESOLUTION_ determines the granularity of generated random numbers. 
    
    
    function randomExp(a, rate: real): real;
    const
      RESOLUTION = 1000;
    var
      unif: real;
    begin
      if rate = 0 then
        randomExp := NaN
      else
      begin
        repeat
          unif := random(RESOLUTION) / RESOLUTION;
        until unif <> 0;
        randomExp := a - rate * ln(unif);
      end;
    end;

## Gamma Distribution

The gamma distribution is a two-parameter family of continuous random distributions. It is a generalization of both the exponential distribution and the Erlang distribution. Possible applications of the gamma distribution include modelling and simulation of waiting lines, or queues, and actuarial science. 

The following function delivers a single real random number out of a gamma distribution. The shape of the distribution is defined by the parameters _a_ , _b_ and _c_. The function makes use of the function **randomExp** as defined above. 
    
    
    function randomGamma(a, b, c: real): real;
    const
      RESOLUTION = 1000;
      T = 4.5;
      D = 1 + ln(T);
    var
      unif: real;
      A2, B2, C2, Q, p, y: real;
      p1, p2, v, w, z: real;
      found: boolean;
    begin
      A2 := 1 / sqrt(2 * c - 1);
      B2 := c - ln(4);
      Q := c + 1 / A2;
      C2 := 1 + c / exp(1);
      found := False;
      if c < 1 then
      begin
        repeat
          repeat
            unif := random(RESOLUTION) / RESOLUTION;
          until unif > 0;
          p := C2 * unif;
          if p > 1 then
          begin
            repeat
              unif := random(RESOLUTION) / RESOLUTION;
            until unif > 0;
            y := -ln((C2 - p) / c);
            if unif <= power(y, c - 1) then
            begin
              randomGamma := a + b * y;
              found := True;
            end;
          end
          else
          begin
            y := power(p, 1 / c);
            if unif <= exp(-y) then
            begin
              randomGamma := a + b * y;
              found := True;
            end;
          end;
        until found;
      end
      else if c = 1 then
        { Gamma distribution becomes exponential distribution, if c = 1 }
      begin
        randomGamma := randomExp(a, b);
      end
      else
      begin
        repeat
          repeat
            p1 := random(RESOLUTION) / RESOLUTION;
          until p1 > 0;
          repeat
            p2 := random(RESOLUTION) / RESOLUTION;
          until p2 > 0;
          v := A2 * ln(p1 / (1 - p1));
          y := c * exp(v);
          z := p1 * p1 * p2;
          w := B2 + Q * v - y;
          if (w + D - T * z >= 0) or (w >= ln(z)) then
          begin
            randomGamma := a + b * y;
            found := True;
          end;
        until found;
      end;
    end;

## Erlang Distribution

The Erlang distribution is a two parameter family of continuous probability distributions. It is a generalization of the exponential distribution and a special case of the gamma distribution, where _c_ is an integer. The Erlang distribution has been first described by Agner Krarup Erlang in order to model the time interval between telephone calls. It is used for queuing theory and for simulating waiting lines. 
    
    
      function randomErlang(mean: real; k: integer): real;
      const
        RESOLUTION = 1000;
      var
        i: integer;
        unif, prod: real;
      begin
        if (mean <= 0) or (k < 1) then
          randomErlang := NaN
        else
        begin
          prod := 1;
          for i := 1 to k do
          begin
            repeat
              unif := random(RESOLUTION) / RESOLUTION;
            until unif <> 0;
            prod := prod * unif;
          end;
          randomErlang := -mean * ln(prod);
        end;
      end;

## Poisson Distribution

The Poisson distribution applies to integer values. It represents the probability of _k_ successes, when the probability of a success in each trial is small and the rate of occurrence (the mean value) is constant. 
    
    
    function randomPoisson(mean: integer): integer;
    { Generator for Poisson distribution (Donald Knuth's algorithm) }
    const
      RESOLUTION = 1000;
    var
      k: integer;
      b, l: real;
    begin
      assert(mean > 0, 'mean < 1');
      k := 0;
      b := 1;
      l := exp(-mean);
      while b > l do
      begin
        k := k + 1;
        b := b * random(RESOLUTION) / RESOLUTION;
      end;
      randomPoisson := k - 1;
    end;

## t Distribution

The t distribution (also referred to a Student's t distribution, since it was published by William Sealy Gosset in 1908 under the pseudonym _Student_) is a continuous probability distribution. Its shape is defined by one parameter, the degrees of freedom (_df_). In statistics, many estimators are t distributed. Therefore, Student's t-distribution plays a major role in a number of widely used statistical analyses, including Student's t-test for assessing the statistical significance of the difference between two sample means, the construction of confidence intervals for the difference between two population means, and in linear regression analysis. The t-distribution also arises in Bayesian analysis of data from a normal family. 

The following algorithm depends on the [RTL](<RTL.md> "RTL") function `[random](<http://lazarus-ccr.sourceforge.net/docs/rtl/system/random.html> "doc:rtl/system/random.html")` and on the **randomChisq** function 
    
    
    function randomT(df: integer): real;
    { Generator for Student's t distribution }
    begin
      if df < 1 then 
        randomT := NaN
      else begin
        randomT := randg(0, 1) / sqrt(randomChisq(df) / df);
      end;
    end;

## Chi Squared Distribution

The chi squared distribution is a continuous distribution of random numbers with _df_ degrees of freedom. It is the distribution of a sum of the squares of _df_ independent standard normal random variables. The chi squared distribution has numerous applications in inferential statistics, e.g. in estimating variances and for chi-squared tests. It is a special gamma distribution with _c_ = _df_ / 2 and _b_ = 2. Therefore the following function depends on the function **randomGamma**. 
    
    
    function randomChisq(df: integer): real;
    begin
      if df < 1 then 
        randomChisq := NaN
      else
        randomChisq := randomGamma(0, 2, 0.5 * df);
    end;

## F Distribution

The F distribution, also referred to as Fisher-Snedecor distribution, is a continuous probability distribution. It is used for F Test and ANOVA. It has two degrees of freedom that serve as shape parameters _v_ and _w_ and that are positive integers. The following function **randomF** makes use of **randomChisq**. 
    
    
    function randomF(v, w: integer): real;
    begin
      if (v < 1) or (w < 1) then
        randomF := NaN
      else
        randomF := randomChisq(v) / v / (randomChisq(w) / w);
    end;

## See also

  * [Dev random](<Dev_random.md> "Dev random")
  * [Functions for descriptive statistics](<Functions_for_descriptive_statistics.md> "Functions for descriptive statistics")
  * [Marsaglia's pseudo random number generators](<Marsaglia's_pseudo_random_number_generators.md> "Marsaglia's pseudo random number generators")
  * [A simple implementation of the Mersenne twister](<A_simple_implementation_of_the_Mersenne_twister.md> "A simple implementation of the Mersenne twister")
  * [Delphi compatible LCG Random](<Delphi_compatible_LCG_Random.md> "Delphi compatible LCG Random")



## References

  1. [G. E. P. Box and Mervin E. Muller, _A Note on the Generation of Random Normal Deviates_ , The Annals of Mathematical Statistics (1958), Vol. 29, No. 2 pp. 610–611](<http://projecteuclid.org/DPubS/Repository/1.0/Disseminate?view=body&id=pdf_1&handle=euclid.aoms/1177706645>)
  2. Dietrich, J. W. (2002). [Der Hypophysen-Schilddrüsen-Regelkreis](<http://openlibrary.org/books/OL24586469M/Der_Hypophysen-Schilddrüsen-Regelkreis>). Berlin, Germany: Logos-Verlag Berlin. ISBN 978-3-89722-850-4. OCLC 50451543. 
  3. Press, W. H., B. P. Flannery, S. A. Teukolsky, W. T. Vetterling (1989). [Numerical Recipes in Pascal](<http://openlibrary.org/works/OL16807779W/>). The Art of Scientific Computing, Cambridge University Press, ISBN 0-521-37516-9. 
  4. Richard Saucier, [Computer Generation of Statistical Distributions](<http://ftp.arl.mil/random/random.pdf>), ARL-TR-2168, US Army Research Laboratory, Aberdeen Proving Ground, MD, 21005-5068, March 2000. 
  5. R.U. Seydel, Generating Random Numbers with Specified Distributions. In: Tools for Computational Finance, Universitext, [DOI 10.1007/978-1-4471-2993-6_2](<http://dx.doi.org/10.1007/978-1-4471-2993-6_2>), © Springer-Verlag London Limited 2012 
  6. Christian Walck, Hand-book on STATISTICAL DISTRIBUTIONS for experimentalists, Internal Report SUF–PFY/96–01, University of Stockholm 2007

---

_Source: [https://wiki.freepascal.org/Generating_Random_Numbers](https://web.archive.org/web/20170920232411/https://wiki.freepascal.org/Generating_Random_Numbers)_
