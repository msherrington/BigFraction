BigFraction
===========

Java library that represents a fraction as a ratio of two `BigInteger`s, reduced to lowest terms. A rich set of mathematical functions are supported.

## Creating BigFractions

Constructors are protected. Create new BigFractions using `valueOf(Number numerator)`, `valueOf(Number numerator, Number denominator)`, or `valueOf(String)`:

    BigFraction.valueOf(11);      // 11/1
    BigFraction.valueOf(11, 17);  // 11/17
    BigFraction.valueOf("19/81"); // 19/81

Fractions are always reduced to lowest terms:

    BigFraction.valueOf(17, 34);  // 1/2
    BigFraction.valueOf(0, 999);  // 0/1

The sign is always carried by the numerator:

    BigFraction.valueOf(-9, 4);   // -9/4
    BigFraction.valueOf(9, -4);   // -9/4
    BigFraction.valueOf(-9, -4);  // 9/4  (negatives cancel out)

You can use floating point numbers to create the fraction:

    BigFraction.valueOf(0.625);  // 5/8
    BigFraction.valueOf(-8.5, 6.25); //-34/25

But be careful, what you get is *exactly* equal to the value you provide:

    BigFraction.valueOf(1.1);  // 2476979795053773/2251799813685248
    BigFraction.valueOf(1.1f); // 9227469/8388608

The version that takes a String may be more like what you expect:

    BigFraction.valueOf("1.1");                // 11/10
    BigFraction.valueOf(Double.toString(1.1)); // 11/10
    BigFraction.valueOf(Float.toString(1.1f)); // 11/10

You can also use `BigInteger` and `BigDecimal`:

    BigFraction.valueOf(new BigInteger("9999999999999999999"), BigInteger.valueOf(1));
    // ->  9999999999999999999/1   (note that this is larger than Long.MAX_VALUE)
    
    BigFraction.valueOf(new BigDecimal("1.23456789012345678901E-50"));
    // ->  123456789012345678901/10000000000000000000000000000000000000000000000000000000000000000000000

You can even mix different `Number` types for numerator and denominator:

    BigFraction.valueOf(1.5, BigInteger.valueOf(17)); // 3/34

A few exceptions:

    BigFraction.valueOf(1,0); // ArithmeticException - divide by zero
    BigFraction.valueOf(0,0); // ArithmeticException - divide by zero
    BigFraction.valueOf(Double.POSITIVE_INFINITY); //IllegalArgumentException
    BigFraction.valueOf(Double.NaN); //IllegalArgumentException

## Mathematical Operations

    BigFraction a = BigFraction.valueOf(1,2);
    BigFraction b = BigFraction.valueOf(3,4);
    BigFraction z = BigFraction.ZERO;
    
    a.add(a); // 1/2 + 1/2 = 1/1
    a.add(b); // 1/2 + 3/4 = 5/4
    b.add(z); // 3/4 + 0/1 = 3/4
    z.add(a); // 0/1 + 1/2 = 1/2
    
    a.subtract(a); // 1/2 - 1/2 = 0/1
    a.subtract(b); // 1/2 - 3/4 = -1/4
    b.subtract(z); // 3/4 - 0/1 = 3/4
    z.subtract(a); // 0/1 - 1/2 = -1/2
    
    a.multiply(a); // (1/2) * (1/2) = 1/4
    a.multiply(b); // (1/2) * (3/4) = 3/8
    b.multiply(z); // (3/4) * (0/1) = 0/1
    z.multiply(a); // (0/1) * (1/2) = 0/1
    
    a.divide(a); // (1/2) / (1/2) = 1/1
    a.divide(b); // (1/2) / (3/4) = 2/3
    b.divide(z); // => ArithmeticException - divide by zero
    z.divide(a); // (0/1) / (1/2) = 0/1
    
    a.pow(3);  // (1/2)^3 = 1/8
    b.pow(4);  // (3/4)^4 = 81/256
    a.pow(0);  // (1/2)^0 = 1/1
    z.pow(0);  // (0/1)^0 = 1/1  => Mathematicians may not like it, but this is consistent with Math.pow()
    
    a.pow(-3); // (1/2)^(-3) = 8/1
    b.pow(-4); // (3/4)^(-4) = 256/81
    z.pow(-1); // => ArithmeticException (divide by zero)
    
    a.reciprocal(); // (1/2)^(-1) = 2/1
    b.reciprocal(); // (3/4)^(-1) = 4/3
    z.reciprocal(); // => ArithmeticException (divide by zero)

Complement is `1 - n`. Useful in statistics a lot:

    a.complement(); // 1 - 1/2 = 1/2
    b.complement(); // 1 - 3/4 = 1/4
    z.complement(); // 1 - 0/1 = 1/1

## LongFraction

There is also a `LongFraction` that represents a fraction as a ratio of two `long`s, with the same methods as `BigFraction`. The mathematics are much faster, but you run the risk of overflows.

## How to Get BigFraction

The library is [available via Maven Central](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22com.github.kiprobinson%22%20AND%20a%3A%22bigfraction%22) and can be used in any project built using Maven or a compatible build tool (Ivy, Gradle, etc.).

You can also download the jar file from the repository and manually add it to your project, if you are not utilizing a supported build tool.

### Support for Older Java Versions

I am currently building my jar files using Java 8, with source compatibility only for Java 8+. These jar files will not run in older versions of Java. If you need support for Java 6 or Java 7, notice that there are `-java6` or `-java7` versions available from Maven. These are compiled with compatibility to that Java version.

