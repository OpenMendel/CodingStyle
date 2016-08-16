Style Guide
========

This style guide is based on John Myles White's [Style.jl](https://github.com/johnmyleswhite/Style.jl) and Hadley Wickham's [R style guide](http://adv-r.had.co.nz/Style.html). These guidelines make explicit what we think are good styles. We'd ask that anyone making contributions to Mendel consider following these guidelines as well.

# Naming Files and Packages

* File names end in `.jl`, except for shell scripts which should not have any explicit file type extension.

* GitHub repo names end in `.jl`.

* Package names *do not* end in `.jl`.

# Whitespace and Line Breaks

* Never use tabs instead of space characters as whitespace in code.

* Use two spaces when indenting:

**Good style**

    function myfunc(n::Integer)
      x = 0
      for i in 1:n
        x += i
      end
      return x
    end

**Bad style**

	function myfunc(n::Integer)
		x = 0
		for i in 1:n
			x = x + i
		end
		return x
	end

* When breaking a long line into multiple lines, indent the remaining lines by two spaces:

**Good style**

    s = a + b + c + d +
      e + f + g + h

**Bad style**

    s = a + b + c + d +
    e + f + g + h

* Never place more than 80 characters on a line.

* Always include a single space after a comma and never insert a space before a comma (as in regular English):

**Good style**

    x[1, 2]

**Bad style**

    x[1,2]
    x[1 , 2]
    x[1 ,2]


* Always insert a single space before and after an operator, except for the `^` and `:` operators, which never have spaces around them:

**Good style**

    1 + 1
    1^2
    1:5

**Bad style**

    1+1
    1 ^ 2
    1 : 5

* The spacing before-and-after rule applies to keyword arguments as well:

**Good style**

    myfunc(a = 1)

**Bad style**

    myfunc(a=1)

* Use explicit parentheses with the `:` operator in complex expressions. Do not rely on Matlab-like precedence rules.

**Good style**

    1:(n - 1)

**Bad style**

    1:n - 1

* Place a space before left parentheses, except in a function call:

**Good style**

    if (debug) do(x)

**Bad style**

    if(debug)do(x)
    plot (x, y)

**Better syle**
 
    if debug do(x)

* Do not place spaces around code in parentheses or square brackets (unless there’s a comma, in which case see above):

**Good style**

    if (debug) do(x)
    diamonds[5, ]

**Bad style**

    if ( debug ) do(x)  # No spaces around debug
    x[1,]   # Needs a space after the comma
    x[1 ,]  # Space goes after comma not before


# Naming Conventions

* When naming variables or functions, use short lowercase names if possible:

**Good style**

    isna

**Bad style**

    isNotAvailable, is_not_available

* If a variable or function name is too long to be read in all lowercase, insert underscores at word boundaries:

**Good style**

    lookup_table

**Bad style**

    lookupTable, LookupTable

* When naming mutable or immutable types, use initial-cap camelcase:

**Good style**

    type Pair
      val1::Float64
      val2::Float64
    end

    immutable ImmutablePair
      val1::Float64
      val2::Float64
    end

**Bad style**

    type pair
      val1::Float64
      val2::Float64
    end

    immutable immutablePair
      val1::Float64
      val2::Float64
    end

    immutable immutable_pair
      val1::Float64
      val2::Float64
    end

* When naming modules, including packages, use initial-cap camelcase, except for acronyms, for which all letters should be capitalized:

**Good style**

    module MyModule
      myfunc(x::Any) = 1
    end

    using MyPackage
    using GLM

**Bad style**

    module myModule
      myfunc(x::Any) = 1
    end

    module my_module
      myfunc(x::Any) = 1
    end

    using my_package
    using myPackage
    using Glm
    using glm

* When naming constants, use all caps:

**Good style**

    const MAGICNUMBER = 1

**Bad style**

    const magicnumber = 1
    const magic_number = 1
    const magicNumber = 1
    const MagicNumber = 1

# Mathematical Notation

* Always add explicit zeros to the ends of floating point constants:

**Good style**

    1.0 + 2.0

**Bad style**

    1. + 2.

* Use unicode via Latex notation for greek letters:

**Good style**

    α, β, γ

**Bad style**

    alpha, beta, gamma


# The Type System

* Always explicitly type all arguments to a function. Explicit typing makes code safer to use and clearer to an unfamiliar user:

**Good style**

    myfunc(x::Real, y::Real; z::Real = 1) = x + y + z

**Bad style**

    myfunc(x, y; z = 1) = x + y + z

* When the desired types for a function are too generic to be tightly typed in Julia, use an explicit `Any`. This makes it clear that you intended for your code to work with any type of input.

**Good style**

    screamcase(x::Any) = uppercase(string(x))

**Bad style**

    screamcase(x) = uppercase(string(x))

* Don't explicitly introduce a parametric type rule for a function unless it's needed to ensure correctness:

**Good style**

    myfunc(x::String) = print(x)
    myfunc(x::Vector) = print(x)
    myfunc{T <: Real}(x::Vector{T}) = sum(x)

**Bad style**

    myfunc{T <: String}(x::T) = print(x)
    myfunc{T <: Any}(x::Vector{T}) = print(x)

* Try to order method definitions from least specific to most specific type constraints.

**Good style**

    myfunc(x::Any) = print(x)
    myfunc(x::String) = print(uppercase(x))

**Bad style**

    myfunc(x::String) = print(uppercase(x))
    myfunc(x::Any) = print(x)

# Performance

* Avoid creating temporary arrays, especially in loops.

* Ensure that functions return a single type for each type signature of inputs.

* Ensure that the type of any variable's binding does not change over the body of a function.

# Code Organization

* Most code should exist in a package, except for isolated scripts. Make ad hoc packages to organize your own work.

* When writing packages, obey the package organization rules by placing code in `src` and tests in `test`.

# Testing

* Always write a separate test file for every source file you write. Specifically, place the tests for `src/myfunc.jl` in `test/myfunc.jl`.

* The contents of `test/myfunc.jl` should be surrounded by a module to keep variables from leaking out:

**Good style**

    module TestMyFunc
      @assert myfunc
    end

**Bad style**

    @assert myfunc

* Test the functionality of `src/myfunc.jl` by writing at least one test for every type/function definition in `src/myfunc.jl`. Ensure systematic code coverage.

* Avoid explicit types for variables inside code unless there is potential for bugs that you need to catch.

**Good style**

    function myfunc()
      x = 1
      return x
    end

**Bad style**

    function myfunc()
      x::Int = 1
      return x
    end

# Comments

* Document functions, types, modules according to the [guide](http://docs.julialang.org/en/release-0.4/manual/documentation/).

* Use `#` to begin each comment line. Leave the first and last line of a comment block empty. Comment lines are indented in the same way as code.

**Good style**

    function myfunc()
      #
      # Define a variable and return it.
      #
      x = 1
      return x
    end

**Bad style**

    function myfunc()
      # define a variable and return it
      x = 1
      return x
    end

    function myfunc()
    # define a variable and return it
      x = 1
      return x
    end

* Avoid over-commenting code. Focus on writing code that makes sense by using informative variable names and simple constructions. If you need to document a non-trivial algorithm or data structure, move that documentation into a specification file where it can be formatted nicely with diagrams and other information. English language documents are much more readable when they're not constrained by the rules for code comments.

* Write separate specification documentation for non-obvious algorithms.

# Error and Warning

* Error messages take the format `ERROR: error message.`

* Warning messages take the format `WARNING: warning message.`

# Be Conservative

Julia often gives you more freedom than you should use. Here are some guidelines for exhibiting self-control in the face of temptation.

* Don't use `importall`. Don't even use `import`. Explicitly annotate the source of each extended function at the point of extension:

**Good style**

    Base.mean(x::MyNewType) = 1.0

**Bad style**

    import Base.median
    median(x::MyNewType) = 1.0

**Worst style**

    importall Base
    median(x::MyNewType) = 1.0

#Further Suggestions for Clarity

* In writing documentation, avoid computer science abbreviations such as "foo" and "bar".

* Julia's C-style updating operators sometimes impede clarity.

**Good style**

    x = x + 1

**Bad style**
 
    x += 1
    
* Julia's abbreviated control structures sometimes impede clarity.

**Good style**

    if x == 1
      println("x is 1")
    else
      println("x is not 1")
    end
    
**Bad style**
 
    x == 1 ? println("x is 1") : println("x is not 1")
 
**Good style**

    if n == 0
      return 1
    end

**Alternative Good style**

    if n == 0; return 1; end   

**Bad style**
 
    n == 0 && return 1
