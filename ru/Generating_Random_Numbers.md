# Generating Random Numbers

│ **[English (en)](<../en/Generating_Random_Numbers.md>)** │  **русский (ru)** │

[![fpc source logo.png](https://wiki.freepascal.org/images/e/e1/fpc_source_logo.png)](</File:fpc_source_logo.png>)

**Случайные числа** являются важными ресурсами для научных приложений, образования, разработки игр и визуализации. Они играют ключевую роль в численном моделировании. 

Генерируемые алгоритмом случайные числа являются псевдослучайными числами. Они принадлежат (большому) набору повторяющихся чисел, последовательность которых невозможно или, по крайней мере, трудно предсказать. В отличие от Delphi, в котором используется линейный конгруэнтный генератор (см. [Delphi compatible LCG Random](<../en/Delphi_compatible_LCG_Random.md> "Delphi compatible LCG Random")), Free Pascal использует алгоритм MersenneTwister для своей стандартной `[random](<http://lazarus-ccr.sourceforge.net/docs/rtl/system/random.html> "doc:rtl/system/random.html")` функции, определенной в [RTL](<../en/RTL.md> "RTL"). Перед первым использованием генератор случайных чисел FPC должен быть проинициализирован единичным вызовом функции `[randomize](<http://lazarus-ccr.sourceforge.net/docs/rtl/system/randomize.html> "doc:rtl/system/randomize.html")`, которая устанавливает начальное число генератора. Предпочтительнее это делать на этапе запуска программы. 

Кроме того, в системах на основе Unix и Linux доступны виртуальные устройства `[/dev/random](<../en/Dev_random.md> "Dev random")` и `/dev/urandom`. Они генерируют (псевдо) случайные числа на основе оборудования. 

Третий вариант - использовать случайные числа из внешних источников, либо из специализированных аппаратных устройств, либо из общедоступных источников, например. на основе данных радиоактивного распада. 

## Contents

  * 1 Равномерное распределение
  * 2 Нормальное (гауссово) распределение
  * 3 Экспоненциальное распределение
  * 4 Гамма-распределение
  * 5 Распределение Эрланга
  * 6 Распределение Пуассона
  * 7 t-распределение (Стьюдента)
  * 8 Распределение хи-квадрат
  * 9 F-распределение (Фишера)
  * 10 См.также
  * 11 Рекомендации



## Равномерное распределение

Непрерывное равномерное распределение (также называемое прямоугольным распределением) представляет собой семейство симметричных вероятностных распределений. Здесь для каждого члена семьи все интервалы одинаковой длины в поддержке распределения одинаково вероятны. 

Стандартная функция [RTL](<../en/RTL.md> "RTL") `[random](<http://lazarus-ccr.sourceforge.net/docs/rtl/system/random.html> "doc:rtl/system/random.html")` генерирует случайные числа с равномерным распределением. При вызове без параметра `random` выдает псевдослучайное число с плавающей запятой в интервале [0, 1), т.е. 0 <= result < 1\. Если `random` вызывается с аргументом `longint L`, возвращается случайное значение longint в интервале [0, L). 

Дополнительный набор равномерно распределенных генераторов случайных чисел представлен в `[генераторах псевдослучайных чисел Марсальи.](<../en/Marsaglia's_pseudo_random_number_generators.md> "Marsaglia's pseudo random number generators")`  
  
Равномерно распределенные случайные числа полезны не для каждого приложения. Для создания случайных чисел других распределений необходимы специальные алгоритмы. 

## Нормальное (гауссово) распределение

Одним из наиболее распространенных алгоритмов получения нормально распределенных случайных чисел из равномерно распределенных случайных чисел является [преобразование Бокса-Мюллера](<https://ru.wikipedia.org/wiki/%D0%9F%D1%80%D0%B5%D0%BE%D0%B1%D1%80%D0%B0%D0%B7%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5_%D0%91%D0%BE%D0%BA%D1%81%D0%B0_%E2%80%94_%D0%9C%D1%8E%D0%BB%D0%BB%D0%B5%D1%80%D0%B0>). Следующая функция вычисляет распределенные по Гауссу случайные числа: 
    
    
     function rnorm (mean, sd: real): real;
     {Вычисляет гауссовы случайные числа в соответствии с преобразованием Бокса-Мюллера}
      var
       u1, u2: real;
     begin
       u1 := random;
       u2 := random;
       rnorm := mean * abs(1 + sqrt(-2 * (ln(u1))) * cos(2 * pi * u2) * sd);
      end;
    

Тот же алгоритм используется функцией randg [randg](<http://lazarus-ccr.sourceforge.net/docs/rtl/math/randg.html> "doc:rtl/math/randg.html") из модуля [RTL](<../en/RTL.md> "RTL") [math](<http://lazarus-ccr.sourceforge.net/docs/rtl/math/index.html> "doc:rtl/math/index.html"): 
    
    
    function randg(mean,stddev: float): float;
    

## Экспоненциальное распределение

Экспоненциальное распределение часто встречается в реальных задачах. Классическим примером является распределение времени ожидания между независимыми пуассоновскими случайными событиями, например, радиоактивный распад ядер [Press et al. 1989]. 

Следующая функция возвращает одно действительное случайное число из экспоненциального распределения. _Rate_ является обратным к среднему значению, а константа _RESOLUTION_ определяет гранулярность генерируемых случайных чисел. 
    
    
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
    

## Гамма-распределение

Гамма-распределение - это двухпараметрическое семейство непрерывных случайных распределений. Это обобщение как экспоненциального распределения, так и распределения Эрланга. Возможные применения гамма-распределения включают моделирование и имитацию линий ожидания, или очередей, и актуарную(страховую) науку. 

Следующая функция возвращает одно действительное случайное число из гамма-распределения. Форма распределения определяется параметрами _a_ , _b_ и _c_. Функция использует функцию **randomExp** , как определено выше. 
    
    
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
        { Гамма-распределение становится экспоненциальным, если c = 1 }
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
    

## Распределение Эрланга

Распределение Эрланга - это двухпараметрическое семейство непрерывных распределений вероятностей. Это обобщение экспоненциального распределения и частный случай гамма-распределения, где _c_ \- целое число. Распределение Эрланга было впервые описано Агнером Крарупом Эрлангом для моделирования временного интервала между телефонными звонками. Он используется для теории очередей и для моделирования линий ожидания. 
    
    
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
    

## Распределение Пуассона

Распределение Пуассона применяется к целочисленным значениям. Оно представляет вероятность успеха _k_ , когда вероятность успеха в каждом испытании мала, а частота появления (среднее значение) постоянна. 
    
    
    function randomPoisson(mean: integer): integer;
    { Генератор для распределения Пуассона (алгоритм Дональда Кнута) }
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
    

## t-распределение (Стьюдента)

t-распределение (также относится к t-распределению Стьюдента, поскольку оно было опубликовано Уильямом Сили Госсетом в 1908 году под псевдонимом _Student_) - это непрерывное распределение вероятностей. Его форма определяется одним параметром, степенями свободы (_df_). В статистике много оценок являются t-распределением. Таким образом, t-распределение Стьюдента играет главную роль в ряде широко используемых статистических анализов, включая t-критерий Стьюдента для оценки статистической значимости разницы между двумя средними выборками, построение доверительных интервалов для разницы между двумя средними значениями, и в линейном регрессионном анализе. Т-распределение также возникает при Байесовском выводе данных из нормального семейства. 

Следующий алгоритм зависит от функции [RTL](<../en/RTL.md> "RTL") `[random](<http://lazarus-ccr.sourceforge.net/docs/rtl/system/random.html> "doc:rtl/system/random.html")` и от функции **randomChisq**
    
    
    function randomT(df: integer): real;
    { Генератор для распределения Стьюдента }
    begin
      if df < 1 then 
        randomT := NaN
      else begin
        randomT := randg(0, 1) / sqrt(randomChisq(df) / df);
      end;
    end;
    

## Распределение хи-квадрат

Распределение хи-квадрат - это непрерывное распределение случайных чисел со степенями свободы _df_. Это распределение суммы квадратов независимых стандартных нормальных случайных величин. Распределение хи-квадрат имеет множество применений в выводной статистике, например, в оценке дисперсий и для тестов хи-квадрат. Это специальное гамма-распределение с _c_ = _df_ / 2 and _b_ = 2. Поэтому следующая функция зависит от функции **randomGamma**. 
    
    
    function randomChisq(df: integer): real;
    begin
      if df < 1 then 
        randomChisq := NaN
      else
        randomChisq := randomGamma(0, 2, 0.5 * df);
    end;
    

## F-распределение (Фишера)

Распределение F, также называемое распределением Фишера-Снедекора, является непрерывным распределением вероятности. Используется для F-теста(критерия Фишера) и ANOVA(ANalysis Of VAriance, или дисперсионный анализ). Оно имеет две степени свободы, которые служат параметрами формы _v_ и _w_ , и являются положительными целыми числами. Следующая функция **randomF** использует **randomChisq**. 
    
    
    function randomF(v, w: integer): real;
    begin
      if (v < 1) or (w < 1) then
        randomF := NaN
      else
        randomF := randomChisq(v) / v / (randomChisq(w) / w);
    end;
    

## См.также

  * [Dev random](<../en/Dev_random.md> "Dev random")
  * [Functions for descriptive statistics](<../en/Functions_for_descriptive_statistics.md> "Functions for descriptive statistics")
  * [Marsaglia's pseudo random number generators](<../en/Marsaglia's_pseudo_random_number_generators.md> "Marsaglia's pseudo random number generators")
  * [A simple implementation of the Mersenne twister](<../en/A_simple_implementation_of_the_Mersenne_twister.md> "A simple implementation of the Mersenne twister")
  * [Delphi compatible LCG Random](<../en/Delphi_compatible_LCG_Random.md> "Delphi compatible LCG Random")



## Рекомендации

  1. [G. E. P. Box and Mervin E. Muller, _A Note on the Generation of Random Normal Deviates_ , The Annals of Mathematical Statistics (1958), Vol. 29, No. 2 pp. 610–611](<http://projecteuclid.org/DPubS/Repository/1.0/Disseminate?view=body&id=pdf_1&handle=euclid.aoms/1177706645>)
  2. Dietrich, J. W. (2002). [Der Hypophysen-Schilddrüsen-Regelkreis](<http://openlibrary.org/books/OL24586469M/Der_Hypophysen-Schilddrüsen-Regelkreis>). Berlin, Germany: Logos-Verlag Berlin. ISBN 978-3-89722-850-4. OCLC 50451543.
  3. Press, W. H., B. P. Flannery, S. A. Teukolsky, W. T. Vetterling (1989). [Numerical Recipes in Pascal](<http://openlibrary.org/works/OL16807779W/>). The Art of Scientific Computing, Cambridge University Press, ISBN 0-521-37516-9.
  4. Richard Saucier, [Computer Generation of Statistical Distributions](<http://ftp.arl.mil/random/random.pdf>), ARL-TR-2168, US Army Research Laboratory, Aberdeen Proving Ground, MD, 21005-5068, March 2000.
  5. R.U. Seydel, Generating Random Numbers with Specified Distributions. In: Tools for Computational Finance, Universitext, [DOI 10.1007/978-1-4471-2993-6_2](<http://dx.doi.org/10.1007/978-1-4471-2993-6_2>), © Springer-Verlag London Limited 2012
  6. Christian Walck, Hand-book on STATISTICAL DISTRIBUTIONS for experimentalists, Internal Report SUF–PFY/96–01, University of Stockholm 2007

---

_Source: [https://wiki.freepascal.org/Generating_Random_Numbers/ru](https://web.archive.org/web/20250418105634/https://wiki.freepascal.org/Generating_Random_Numbers/ru)_
