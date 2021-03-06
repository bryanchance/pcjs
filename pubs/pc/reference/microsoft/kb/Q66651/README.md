---
layout: page
title: "Q66651: _far16 Generated by Mistake for Huge with /Zg"
permalink: /pubs/pc/reference/microsoft/kb/Q66651/
---

## Q66651: _far16 Generated by Mistake for Huge with /Zg

	Article: Q66651
	Version(s): 6.00   | 6.00
	Operating System: MS-DOS | OS/2
	Flags: ENDUSER | buglist6.00 fixlist6.00a
	Last Modified: 1-NOV-1990
	
	If the /Zg switch is used with the Microsoft C version 6.00 compiler,
	it will erroneously generate _far16 where huge is required.
	
	Microsoft has confirmed this to be a problem with the C version 6.00
	compiler. This problem has been corrected in the version 6.00a
	maintenance release.
	
	The /Zg switch causes the compiler to generate prototypes for all the
	functions in the module. If a function is declared as
	
	   char huge * foo( char huge *){}
	
	the compiler will generate the following prototype:
	
	   char _far16 * foo (char _far16 *);
	
	Upon subsequent compiling with the generated prototype, the compiler
	will fail with various syntax errors because _far16 is an unknown
	keyword.
