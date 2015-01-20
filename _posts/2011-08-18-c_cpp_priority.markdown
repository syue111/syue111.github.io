---
author: dashashi
comments: true
date: 2011-08-18 12:50:56+00:00
layout: post
slug: cc%e4%bc%98%e5%85%88%e7%ba%a7%e5%88%97%e8%a1%a8
title: C/C++优先级列表
wordpress_id: 1346
categories:
- 参考资料
---

C的优先级有时候会发生很坑爹的问题- -贴个优先级表方便查看- -

有个C的，有个C++的，老实说没发现什么区别- -|||<!-- more -->


## C优先级列表


<table cellpadding="2" width="671" cellspacing="0" border="1" >
<tbody >
<tr >

<td width="126" valign="top" >**Precedence**
</td>

<td width="106" valign="top" >**Operator**
</td>

<td width="151" valign="top" >**Description**
</td>

<td width="156" valign="top" >**Example**
</td>

<td width="130" valign="top" >**Associativity**
</td>
</tr>
<tr >

<td width="128" valign="top" >1
</td>

<td width="107" valign="top" >()
[]
->
.
::
++
--
</td>

<td width="149" valign="top" >Grouping operator
Array access
Member access from a pointer
Member access from an object
Scoping operator
Post-increment
Post-decrement
</td>

<td width="154" valign="top" >(a + b) / 4;
array[4] = 2;
ptr->age = 34;
obj.age = 34;
Class::age = 2;
for( i = 0; i < 10; i++ ) ...
for( i = 10; i > 0; i-- ) ...
</td>

<td width="131" valign="top" > 



left to right
</td>
</tr>
<tr >

<td width="129" valign="top" >2
</td>

<td width="108" valign="top" >!
~
++
--
-
+
*
&
(type)
[sizeof](http://www.cppreference.com/keywords/sizeof.html)
</td>

<td width="149" valign="top" >Logical negation
Bitwise complement
Pre-increment
Pre-decrement
Unary minus
Unary plus
Dereference
Address of
Cast to a given type
Return size in bytes
</td>

<td width="154" valign="top" >if( !done ) ...
flags = ~flags;
for( i = 0; i < 10; ++i ) ...
for( i = 10; i > 0; --i ) ...
int i = -1;
int i = +1;
data = *ptr;
address = &obj;
int i = (int) floatNum;
int size = sizeof(floatNum);
</td>

<td width="132" valign="top" >






right to left
</td>
</tr>
<tr >

<td width="130" valign="top" >3
</td>

<td width="108" valign="top" >->*
.*
</td>

<td width="148" valign="top" >Member pointer selector
Member pointer selector
</td>

<td width="153" valign="top" >ptr->*var = 24;
obj.*var = 24;
</td>

<td width="132" valign="top" >
left to right
</td>
</tr>
<tr >

<td width="131" valign="top" >4
</td>

<td width="108" valign="top" >*
/
%
</td>

<td width="148" valign="top" >Multiplication
Division
Modulus
</td>

<td width="153" valign="top" >int i = 2 * 4;
float f = 10 / 3;
int rem = 4 % 3;
</td>

<td width="132" valign="top" >left to right
</td>
</tr>
<tr >

<td width="131" valign="top" >5
</td>

<td width="108" valign="top" >+
-
</td>

<td width="148" valign="top" >Addition
Subtraction
</td>

<td width="153" valign="top" >int i = 2 + 3;
int i = 5 - 1;
</td>

<td width="132" valign="top" >left to right
</td>
</tr>
<tr >

<td width="131" valign="top" >6
</td>

<td width="108" valign="top" ><<
>>
</td>

<td width="148" valign="top" >Bitwise shift left
Bitwise shift right
</td>

<td width="153" valign="top" >int flags = 33 << 1;
int flags = 33 >> 1;
</td>

<td width="132" valign="top" >left to right
</td>
</tr>
<tr >

<td width="131" valign="top" >7
</td>

<td width="108" valign="top" ><
<=
>
>=
</td>

<td width="148" valign="top" >Comparison less-than
Comparison less-than-or-equal-to
Comparison greater-than
Comparison geater-than-or-equal-to
</td>

<td width="153" valign="top" >if( i < 42 ) ...
if( i <= 42 ) ...
if( i > 42 ) ...
if( i >= 42 ) ...
</td>

<td width="132" valign="top" > 



left to right
</td>
</tr>
<tr >

<td width="131" valign="top" >8
</td>

<td width="108" valign="top" >==
!=
</td>

<td width="148" valign="top" >Comparison equal-to
Comparison not-equal-to
</td>

<td width="153" valign="top" >if( i == 42 ) ...
if( i != 42 ) ...
</td>

<td width="132" valign="top" >left to right
</td>
</tr>
<tr >

<td width="131" valign="top" >9
</td>

<td width="108" valign="top" >&
</td>

<td width="148" valign="top" >Bitwise AND
</td>

<td width="153" valign="top" >flags = flags & 42;
</td>

<td width="132" valign="top" >left to right
</td>
</tr>
<tr >

<td width="131" valign="top" >10
</td>

<td width="108" valign="top" >^
</td>

<td width="148" valign="top" >Bitwise exclusive OR
</td>

<td width="153" valign="top" >flags = flags ^ 42;
</td>

<td width="132" valign="top" >left to right
</td>
</tr>
<tr >

<td width="131" valign="top" >11
</td>

<td width="108" valign="top" >|
</td>

<td width="148" valign="top" >Bitwise inclusive (normal) OR
</td>

<td width="153" valign="top" >flags = flags | 42;
</td>

<td width="132" valign="top" >left to right
</td>
</tr>
<tr >

<td width="131" valign="top" >12
</td>

<td width="108" valign="top" >&&
</td>

<td width="148" valign="top" >Logical AND
</td>

<td width="153" valign="top" >if( conditionA && conditionB ) ...
</td>

<td width="132" valign="top" >left to right
</td>
</tr>
<tr >

<td width="131" valign="top" >13
</td>

<td width="108" valign="top" >||
</td>

<td width="148" valign="top" >Logical OR
</td>

<td width="153" valign="top" >if( conditionA || conditionB ) ...
</td>

<td width="132" valign="top" >left to right
</td>
</tr>
<tr >

<td width="131" valign="top" >14
</td>

<td width="108" valign="top" >? :
</td>

<td width="148" valign="top" >Ternary conditional (if-then-else)
</td>

<td width="153" valign="top" >int i = (a > b) ? a : b;
</td>

<td width="132" valign="top" >right to left
</td>
</tr>
<tr >

<td width="131" valign="top" >15
</td>

<td width="108" valign="top" >=
+=
-=
*=
/=
%=
&=
^=
|=
<<=
>>=
</td>

<td width="148" valign="top" >Assignment operator
Increment and assign
Decrement and assign
Multiply and assign
Divide and assign
Modulo and assign
Bitwise AND and assign
Bitwise exclusive OR and assign
Bitwise inclusive (normal) OR and assign
Bitwise shift left and assign
Bitwise shift right and assign
</td>

<td width="153" valign="top" >int a = b;
a += 3;
b -= 4;
a *= 5;
a /= 2;
a %= 3;
flags &= new_flags;
flags ^= new_flags;
flags |= new_flags;
flags <<= 2;
flags >>= 2;
</td>

<td width="132" valign="top" > 









right to left
</td>
</tr>
<tr >

<td width="131" valign="top" >16
</td>

<td width="108" valign="top" >,
</td>

<td width="148" valign="top" >Sequential evaluation operator
</td>

<td width="154" valign="top" >for( i = 0, j = 0; i < 10; i++, j++ ) ...
</td>

<td width="133" valign="top" >left to right
</td>
</tr>
</tbody>
</table>


# C++优先级列表



<table cellpadding="2" width="671" cellspacing="0" border="1" >
<tbody >
<tr >

<td width="128" valign="top" >**Precedence**
</td>

<td width="106" valign="top" >**Operator**
</td>

<td width="150" valign="top" >**Description**
</td>

<td width="154" valign="top" >**Example**
</td>

<td width="131" valign="top" >**Associativity**
</td>
</tr>
<tr >

<td width="129" valign="top" >1
</td>

<td width="107" valign="top" >()
[]
->
.
::
++
--
</td>

<td width="149" valign="top" >Grouping operator
Array access
Member access from a pointer
Member access from an object
Scoping operator
Post-increment
Post-decrement
</td>

<td width="153" valign="top" >(a + b) / 4;
array[4] = 2;
ptr->age = 34;
obj.age = 34;
Class::age = 2;
for( i = 0; i < 10; i++ ) ...
for( i = 10; i > 0; i-- ) ...
</td>

<td width="132" valign="top" >





left to right
</td>
</tr>
<tr >

<td width="130" valign="top" >2
</td>

<td width="108" valign="top" >!
~
++
--
-
+
*
&
(type)
[sizeof](http://www.cppreference.com/keywords/sizeof.html)
</td>

<td width="148" valign="top" >Logical negation
Bitwise complement
Pre-increment
Pre-decrement
Unary minus
Unary plus
Dereference
Address of
Cast to a given type
Return size in bytes
</td>

<td width="152" valign="top" >if( !done ) ...
flags = ~flags;
for( i = 0; i < 10; ++i ) ...
for( i = 10; i > 0; --i ) ...
int i = -1;
int i = +1;
data = *ptr;
address = &obj;
int i = (int) floatNum;
int size = sizeof(floatNum);
</td>

<td width="133" valign="top" >






right to left
</td>
</tr>
<tr >

<td width="131" valign="top" >3
</td>

<td width="108" valign="top" >->*
.*
</td>

<td width="148" valign="top" >Member pointer selector
Member pointer selector
</td>

<td width="152" valign="top" >ptr->*var = 24;
obj.*var = 24;
</td>

<td width="134" valign="top" >left to right
</td>
</tr>
<tr >

<td width="132" valign="top" >4
</td>

<td width="108" valign="top" >*
/
%
</td>

<td width="148" valign="top" >Multiplication
Division
Modulus
</td>

<td width="151" valign="top" >int i = 2 * 4;
float f = 10 / 3;
int rem = 4 % 3;
</td>

<td width="134" valign="top" >left to right
</td>
</tr>
<tr >

<td width="132" valign="top" >5
</td>

<td width="108" valign="top" >+
-
</td>

<td width="148" valign="top" >Addition
Subtraction
</td>

<td width="151" valign="top" >int i = 2 + 3;
int i = 5 - 1;
</td>

<td width="134" valign="top" >left to right
</td>
</tr>
<tr >

<td width="132" valign="top" >6
</td>

<td width="108" valign="top" ><<
>>
</td>

<td width="148" valign="top" >Bitwise shift left
Bitwise shift right
</td>

<td width="151" valign="top" >int flags = 33 << 1;
int flags = 33 >> 1;
</td>

<td width="134" valign="top" >left to right
</td>
</tr>
<tr >

<td width="132" valign="top" >7
</td>

<td width="108" valign="top" ><
<=
>
>=
</td>

<td width="148" valign="top" >Comparison less-than
Comparison less-than-or-equal-to
Comparison greater-than
Comparison geater-than-or-equal-to
</td>

<td width="151" valign="top" >if( i < 42 ) ...
if( i <= 42 ) ...
if( i > 42 ) ...
if( i >= 42 ) ...
</td>

<td width="134" valign="top" >



left to right
</td>
</tr>
<tr >

<td width="132" valign="top" >8
</td>

<td width="108" valign="top" >==
!=
</td>

<td width="148" valign="top" >Comparison equal-to
Comparison not-equal-to
</td>

<td width="151" valign="top" >if( i == 42 ) ...
if( i != 42 ) ...
</td>

<td width="134" valign="top" >left to right
</td>
</tr>
<tr >

<td width="132" valign="top" >9
</td>

<td width="108" valign="top" >&
</td>

<td width="148" valign="top" >Bitwise AND
</td>

<td width="151" valign="top" >flags = flags & 42;
</td>

<td width="134" valign="top" >left to right
</td>
</tr>
<tr >

<td width="132" valign="top" >10
</td>

<td width="108" valign="top" >^
</td>

<td width="148" valign="top" >Bitwise exclusive OR
</td>

<td width="151" valign="top" >flags = flags ^ 42;
</td>

<td width="134" valign="top" >left to right
</td>
</tr>
<tr >

<td width="132" valign="top" >11
</td>

<td width="108" valign="top" >|
</td>

<td width="148" valign="top" >Bitwise inclusive (normal) OR
</td>

<td width="151" valign="top" >flags = flags | 42;
</td>

<td width="134" valign="top" >left to right
</td>
</tr>
<tr >

<td width="132" valign="top" >12
</td>

<td width="108" valign="top" >&&
</td>

<td width="148" valign="top" >Logical AND
</td>

<td width="151" valign="top" >if( conditionA && conditionB ) ...
</td>

<td width="134" valign="top" >left to right
</td>
</tr>
<tr >

<td width="132" valign="top" >13
</td>

<td width="108" valign="top" >||
</td>

<td width="148" valign="top" >Logical OR
</td>

<td width="151" valign="top" >int i = (a > b) ? a : b
</td>

<td width="134" valign="top" >left to right
</td>
</tr>
<tr >

<td width="132" valign="top" >14
</td>

<td width="108" valign="top" >? :
</td>

<td width="148" valign="top" >Ternary conditional (if-then-else)
</td>

<td width="151" valign="top" >if( conditionA || conditionB ) ...
</td>

<td width="134" valign="top" >right to left
</td>
</tr>
<tr >

<td width="132" valign="top" >15
</td>

<td width="108" valign="top" >=
+=
-=
*=
/=
%=
&=
^=
|=
<<=
>>=
</td>

<td width="148" valign="top" >Assignment operator
Increment and assign
Decrement and assign
Multiply and assign
Divide and assign
Modulo and assign
Bitwise AND and assign
Bitwise exclusive OR and assign
Bitwise inclusive (normal) OR and assign
Bitwise shift left and assign
Bitwise shift right and assign
</td>

<td width="151" valign="top" >int a = b;
a += 3;
b -= 4;
a *= 5;
a /= 2;
a %= 3;
flags &= new_flags;
flags ^= new_flags;
flags |= new_flags;
flags <<= 2;
flags >>= 2
</td>

<td width="134" valign="top" >










right to left
</td>
</tr>
<tr >

<td width="132" valign="top" >16
</td>

<td width="108" valign="top" >,
</td>

<td width="148" valign="top" >Sequential evaluation operator
</td>

<td width="152" valign="top" >for( i = 0, j = 0; i < 10; i++, j++ ) ...
</td>

<td width="135" valign="top" >left to right
</td>
</tr>
</tbody>
</table>
