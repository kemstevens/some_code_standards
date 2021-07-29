
Coding Conventions for an SST Project
=======================================================

SST projects require C++, Python, and (sometimes) JSON.  This document is a very brief code standard for an SST project.

## General Approach

Two ground rules here:
1. Please make sure your code compiles with **no warnings** before you commit.  Compile as C++14 with flag -Wall.
2. Write comments as you go.  All functions/methods/classes should have documentation.


## Header File Comments

Formally comment every C++ class in the header file, like so:

		/*!
		  @brief Here is the purpose of my class.
		  Here is some detail on how to use it.
		  @deprecated see class MyNewVersion because of such and such a version.
		 */
		MyClass {
		}

Similarly, comment every python class per pep-8.

## Function Comments

Example:
		/*!
		  @brief This function computes X.
		  More detailed explanation.
		  @param bar specifies what X to compute.
		  @throws an exception is bar has invalid value Y.
		  @return X or some special return value Z in the case of blah blah.
		  @deprecated use function fbr instead because of ABC.
		 */
		int foo(Bar *bar);

Do these formal C++ comments go in the hpp file or the cpp file?  The go in the **header** file if it is a public
function.  You can opt to put them in the cpp if it is not public.

Similarly, comment every python function with complete meta information, like so:

		def my_func:(param_1, param_2):
			"""
			@brief Here is a brief description of my function.
			Here is some more information about it.
			@param param_1 possibly-empty list used for X purpose.
			@param param_2 the Y parameter, should be a Number.
			@throws exception if param_2 is such and such.
			@return a string with such and such a purpose.
			"""
			return "hello world"

**In python the burden on documentation is even higher because class types are not readily available.**  The
developer should document the expectations for parameter types, valid values, and any side effects the function may have
on a parameter's content.  Similarly, document the return value's type and meaning.
Expected exceptions, error conditions, and deprecation should also be documented.

## Namespace Comments
If you write a formal comment for your namespace, doxygen will pick up and and give you a pretty class list page
for your namespace.  All you need to do is put a formal comment in front of one of your
"using namespace XYZ" declarations, like so:

		//! @brief Provides XYZ capability.
		using namespace MyPackage {

## Naming Conventions

Readability always comes first.  If in your judgement a guideline doesn't improve readability, then do what is best for readability instead.  

We give below some guidance on capitalization, but this is a relatively minor concern.  Of much higher concern is having descriptive names
that match the way you discuss and present your code.  If your code is using an out-dated name, that's a much higher priority to fix.
Also, beware of name overloading in the context of the broader project.  Try to avoid re-using the name your neighbor picked if you are
referring to an object that is fundamentally different.  Do not sabotage the ability of someone new to learn your neighbor's
software package.

C++
* NamespaceNames
* ClassAndEnumNames
* function_names
* variable_names
* ENUM_VALUES_AND_CONSTANTS  
* Files: use ClassName.hpp so you always match the class name to the file.

Python
* NamespaceNames
* ClassAndEnumNames
* function_names
* variable_names
* ENUM_VALUES_AND_CONSTANTS  
* Files: use ClassName.py for major python classes.  Otherwise, a DescriptiveScriptName.py is fine.


JSON key strings
- "ComponentsOrBroadCategoriesLikeThis"
- for common acronyms, all caps is "OK"
- if your SST component was not named LikeThis, then make sure the string matches the class name of the component no matter what
- "MAGIC_COMMAND_STRINGS"
- "parameters_for_a_component"

# Code Formatting

For C++, clang-format is used.  You will need at least version 7.0 to run with our clang-format file.  

		clang-format -assume-filename=clang-format-file -i *.*pp


Format python files with black, like so:

		black --line-length 120 -t py36 *.py

Please consider the git log history when formatting.  Only run an auto-formatter when you have no local
uncommitted changes and always commit reformatting changes with a useful message like: 'formatting only.'
We separate major formatting-only commits from substantive feature development so that future
log reviewers will be able to find the changes that are meaningful and ignore the changes that are
purely eye candy.

## Other Random Bits
Includes should use the full path, like:

		#include "MySoftware/PackageName/Class.hpp"

This also goes for python:

		import MySoftware.DirectoryName.PythonFileName

Namespaces should match the directory path, so namespace "MySoftware::Foo" goes in directory MySoftware/Foo
with include "MySoftware/Foo/Class.hpp".

The first include in a cpp file should be the corresponding header file.

For header files, it's cleaner to use:

		#pragma once

instead of:

		#ifndef UNIQUE_STRING
		#define UNIQUE_STRING
		<snip>
		#endif UNIQUE_STRING

However, if you do use a UNIQUE_STRING, if should match the directory and file you are at, like so:

		#ifndef MySoftware_Foo_Class_hpp

Don't make liberal use of "auto" because it conceals the class type.  It makes life very difficult
for future readers.  If the class type is not completely obvious from the context,
then take the time to write out the full type name.
