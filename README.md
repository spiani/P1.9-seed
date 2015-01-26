Programming "by Contract"
=========================

The contract
------------

- Create a library, with the following structure:

    - ./include/
    - ./source/
    - ./cmake/
    - ./tests/

- The library should be self-contained, in its own namespace,
  only allowed dependencies are stl libraries (C++98)

- It should perform integration on the unit tensor product element

- It should be 'object oriented', meaning that there should be one 
  abstract class, templated on the dimension of the unit box, 
  instantiated for dim = 1, 2, 3, defining *only* the interface of 
  the function
    
    - `Quadrature<dim> quad; // Empty constructor`
    - `Quadrature<dim> quad(points, weights); // full constructor`

- And there should be some trivial implementations, like 
  TrapezQuadrature, or SimpsonQuadrature, or TensorProductQuadrature

- When compiled, there should be two objects: libquadrature.debug,
  and libquadrature.release, which you can link to your programs
  with -lquadrature.debug or -lquadrature.release (after specifying
  correctly -L and -I options)

- The library should strictly follow this coding conventions: 
  https://www.dealii.org/developer/doxygen/deal.II/CodingConventions.html

- The debug version should perform extensive checking in input, 
  output, and internal conditions, while the release version should 
  not perform any checking (used only for production runs)

- Each component and function of the library should be documented 
  *first*, a *unit test* should be written in ./tests, and ctest
  should be used to launch all tests, submitting results to cdash

- All unit tests should be written *before* the actual 
  implementation. Example: integrate a linear function between 0 and
  1 and check that the results is exactly what you get from 
  TrapezQuadrature<1>

- The cmake directory should contain cmake include files, include
  should contain *only* the definition of the C++ functions,
  and source should contain only the implementation of the functions

- When the library is installed, only include files and the library
  itself should be copied over

- The directory tests should contain pair of files:

    - unit_test_01.cc
    - unit_test_01.output

- The test is considered passed if the actual output of the program
  matches the one contained in the .output file, using **numdiff**, 
  to avoid numerical roundoff

- Add a .travis.yml file to the repository, and make sure that at 
  every pull request, the full testuite is run

- Design decisions are taken together, at the early stage of the 
  project, when only documentation and test unit is available

- Documentation should be *extracted* automatically from code using
  Doxygen (or equivalent tool)
