# More About Data Types

## The Container Types

Some of the data types in Pike are **containers**. A data item of a container type can contain zero or more other data items. The container types are also **reference types**: When a data item of a reference type is stored in a variable, it is not the data item itself that is stored, but a reference to it. This means that multiple variables can refer to the same data item, and a change in one is seen by all because they're all pointing to the same item. It also means that multiple data items can look identical but not be the same, so be careful! We will, of course discuss this detail more shortly.

These are the **container types** in Pike:

*   array or "vector" (written array in Pike)
    
*   mapping, "dictionary" or "associative array" (written mapping)

*   multiset or "bag" (written multiset)


The Data Type array
-------------------

The simplest of the container types is the **array**. If a variable is a box where you can put a data item, an array is a whole sequence of such boxes. The boxes are numbered, starting with 0, so in an array with 10 places the first one has number 0, and the tenth and last one has number 9. The position numbers are called **indices**. (One **index**, many indices.)

You can specify an array in your Pike program by listing its elements inside parenthesis-curly-bracket quotes:

```pike
({ "geegle", "boogle", "fnordle blordle" })
({ 12, 17, -3, 8 })
({  })
({ 12, 17, 17, "foo", ({ "gleep", "gleep" }) })
```

As you can see, an array can be empty (containing no elements), and it can also contain other arrays. An array can contain elements of different types, but it is more common that arrays only contain one type of element.

You can define array variables like this:

```pike
array a;		// Array of anything
array(string) b;	// Array of strings
array(array(string)) c;	// Array of arrays of strings
array(mixed) d;		// Array of anything
```

As you can see, array literals are written as comma-separated lists inside parenthesis-curly-bracket quotes. The data type of an array that can contain elements of the data type _datatype_ is array(_datatype_). The data type array(mixed), i. e. an array that can contain any types of values, can also be written just array.

An array variable that hasn't been given a value contains 0, and not an empty array. If you want an empty array, you have to give it explicitly:

```pike
array(string) e;       // e contains 0
e = ({ });             // Now e contains an empty array
array(int) f = ({ });  // f contains an empty array
```

Then you can give values to these variables:

```pike
a = ({ 17, "hello", 3.6 });
b = ({ "foo", "bar", "fum" });
c = ({ ({ "John", "Sherlock" }), ({ "Holmes" }) });
d = c; // d and c now point to the same array
```

### Indexing

You can access the elements in an array, either to just get the value or to replace it. This is usually called **indexing** the array. Indexing is done by writing the position number, or index, within square brackets after the array:

```pike
write(a[0]);	// Writes 17
b[1] = "bloo";	// Replaces "bar" with "bloo"
c[1][0] = b[2];	// Replaces "Holmes" with "fum"
```

Note that the first position in an array is numbered 0 and not 1, and the second one is numbered 1, and so on.

A special feature is that you can use negative indices: _array_[-1] means the last position in the array _array_, _array_[-2] the next-to-last position, and so on.

As we mentioned earlier, an array can potentially contain any type of values, including other arrays. In that case, you may need several indexing operators after each other:

```pike
array(array(int)) aai = ({
  ({ 7, 9, 8 }),
  ({ -4, 9 }),
  ({ 100, 1, 2, 4, 17 })
});

write("aai[2][3] is " + aai[2][3] + "\\n");
```

This will print `aai[2][3] is 4`.

### Same versus Equal

As we discussed in the introduction to this section, container types, being reference types have some unique properties that make them behave differently from variables of type int, float and string. Namely, that variable of type array, for example:

*   refers to an array that's stored somewhere in the memory of our program
*   multiple variables can point to the same array
*   multiple arrays that are distinct but look the same can exist at the same time

Thus, it is sometimes important to differentiate between two array expressions being **equal**, and two that also are the **same**. Whenever you write an array literal in your program, you get a new array. This array is only the **same** as itself, but it can be **equal** to other arrays. After executing the following code snippet, the variables a and b will refer to the **same** array, but c will refer to an array that is just **equal** to the first one.

```pike
array(string) a = ({ "foo", "bar" });
array(string) b = a; 
array(string) c = ({ "foo", "bar" }); 
```

### Array Operations

Here are some of the many things that you can do with arrays:

*   **Check if it is an array**  
    `arrayp(something)`
    
    The function `arrayp` returns **1** if the value _something_ is an array, otherwise **0**.
    

*   **Extract a range**  
    
    `array[from..to]` returns a new array, containing the elements at the index _from_ up to and including the index _to_.
    
    `({ 1, 7, 3, 3, 7 })[ 1..3 ]` gives the result `({ 7, 3, 3 })`.
    
    The form `array[from..]` will give the elements starting at index _from_ and to the end of the array. The form `array[..to]` will give the elements from the start of the array, up to and including index _to_.
    

*   **Comparing arrays**  
    
    `array1 == array2` returns **1** if _array1_ and _array2_ are the **same** array, otherwise **0**. They have to be the _same_ array, not just equal in terms of its contents and ordering. Given the variable definition
    
    `array(int) a = ({ 7, 1 });`
    
    this will be true:
    
    `a == a`
    
    but this will be false:
    
    `({ 7, 1 }) == ({ 7, 1 })`
    
    You can also use the operator `!=`, which means "not same". The relational operators (`<`, `>`, etc) do not work with arrays.
    

*   **Comparing arrays (again)**  
    
    `equal(array1, array2)` returns **1** if the contents of _array1_ and _array2_ are **equal**, otherwise **0**. Two arrays look the same if they have the same number of elements, and each two corresponding elements in the two arrays look the same. For example, this will be return 1:
    
    `equal( ({ 7, 1 }), ({ 7, 1 }) );`
    
    It's worth noting that comparing an array with itself using either `equal()` or `==` will always return **1** because an array is always the same as and equal to itself. It's frequently important to understand which of the answers you need because arrays are **reference types**, and picking the wrong one can be the source of hard to trace bugs! 

*   **Concatenation**  
    
    `array1 + array2` returns a new array with the elements from both arrays, in the same order. This is a simple concatenation of the arrays, so duplicate elements are of course not removed.
    
    `({ 7, 1, 1 }) + ({ 1, 3 })` gives the result `({ 7, 1, 1, 1, 3 })`.
    

*   **Union**  
    
    `array1 | array2` returns a new array with the elements that are present in _array1_, or in _array2_, or in both. The elements in the result can come in any order, and duplicates may or may not be removed.
    
    `({ 7, 1 }) | ({ 3, 1 })` gives the result `({ 7, 3, 1 })`.
    

*   **Intersection**  
    
    `array1 & array2` returns a new array with the elements that are present in both arrays. The elements in the result can come in any order, and duplicates may or may not be removed.
    
    `({ 7, 1 }) & ({ 3, 1 })` gives the result `({ 1 })`.
    

*   **Difference**  
    
    `array1 - array2` returns a new array with the elements in the array _array1_ that are not also present in the array _array2_. The elements in the result can come in any order, and duplicates may or may not be removed.
    
    `({ 7, 1 }) - ({ 3, 1 })` gives the result `({ 7 })`.
    

*   **Exclusive or**  
    
    `array1 ^ array2` returns a new array with the elements that are present in _array1_ or in _array2_, but not in both. The elements in the result can come in any order, and duplicates may or may not be removed.
    
    `({ 7, 1 }) ^ ({ 3, 1 })` gives the result `({ 7, 3 })`.
    

*   **Division**  
    `array / delimiter`
    
    This will split the array _array_ into an array of arrays. If the _delimiter_ is an array, the array _array_ will be split at each occurrence of that array:
    
    `({ 7, 1, 2, 3, 4, 1, 2, 1, 2, 77 }) / ({ 1, 2 })` gives the result `({ ({ 7 }), ({ 3, 4 }), ({ }), ({ 77 }) })`.
    
    If the _delimiter_ is an **integer**, the array _array_ will be split into arrays of size _delimiter_, with any extra elements ignored:
    
    `({ 7, 1, 2, 3, 4, 1, 2 })` / 3 gives the result `({ ({ 7, 1, 2 }), ({ 3, 4, 1 }) })`.
    
    If you convert the same integer to a **floating-point number**, the extra elements will not be thrown away:
    
    `({ 7, 1, 2, 3, 4, 1, 2 }) / 3.0` gives the result `({ ({ 7, 1, 2 }), ({ 3, 4, 1 }), ({ 2 }) })`.
    

*   **Modulo**  
    `array % integer`
    
    This gives the extra elements that would be ignored in the division operation `array / integer`:
    
    `({ 7, 1, 2, 3, 4, 1, 2 }) % 3` gives the result `({ 2 })`.
    

*   **Finding the size**  
    
    `sizeof(array)` returns the number of elements in the array _array_.
    
    `sizeof( ({ }) )` gives the result `0`.
    

*   **Allocating an empty array**  
    `allocate(size)`
    
    This will create an array with _size_ elements. _size_ is an integer. All the elements will have the value **0**.
    

*   **Reversing an array**  
    
    `reverse(array)` returns a new array with the elements in the array _array_ in reverse order: with the first element last, and so on. This operation creates a copy, and does not change the array _array_ itself.
    

*   **Finding an element in an array**  
    
    `search(haystack, needle)` returns the index of the first occurrence of an element equal to the _needle_ in the array _haystack_. If the needle wasn't found `-1` will be returned. The comparison is done with `==`, so the element must be the **same** as the _needle_.
    
    If only the existence of an element in an array is needed, the function `has_value(haystack, needle)` can be used instead. It returns 1 on success and 0 on failure.
    

*   **Replacing elements in an array**  
    
    `replace(array, old, new)` replaces all the elements that are equal (with \==) to _old_ with _new_. This operation does not create a copy, but changes the array _array_ itself.
    
    
The Data Type mapping
---------------------

**Mappings** are sometimes called dictionaries or associative arrays. A mapping lets you translate from one value (such as "beer") to another value ("cerveza"). This is possible since the mapping contains **index-value pairs**, consisting of two data items. If you know the **index**, Pike can quickly find the corresponding **value** for you.

A mapping literal can be written as a comma-separated list of index-value pairs inside parenthesis-square-bracket quotes:

```pike
([ "beer" : "cerveza", "cat" : "gato", "dog" : "perro" ])
```

The data type of a mapping with indices of the type _index-type_ and values of the type _value-type_ is written mapping(_index-type_ : _value-type_). The data type mapping(mixed:mixed), i. e. a mapping that can contain any types of indices and values, can also be written just mapping.

Here are a few variables that can contain mappings:

```pike
mapping(string:string) m;
mapping(int:float) mif = ([ 1:3.6, -19:73.0 ]);
mapping(string:string) english2spanish = ([
  "beer" : "cerveza",
  "cat" : "gato",
  "dog" : "perro"
]);
mapping(mixed:int) m2i = ([ 19.0 : 3, "foo" : 17 ]);
```

A mapping variable that hasn't been given a value contains **0**, and not an empty mapping. If you want an empty mapping, you have to give it explicitly:

```pike
mapping(string:float) m1;  // m1 contains 0
m1 = ([ ]);   // Now m1 contains an empty mapping
mapping(int:int) m2 = ([ ]);
              // m2 contains an empty mapping
```

When you want to **look up** a value in the mapping, you use the same **indexing operator** as for arrays: write the index within square brackets after the mapping. You can use this both to just retrieve values, and to change them:

```pike
write(english2spanish["cat"]); // Prints "gato"
english2spanish["dog"] = "gato";
    // Now, english2spanish["dog"] is "gato" too
english2spanish["beer"] = english2spanish["cat"];
    // Now, all values are "gato"
```

Index-value pairs can be inserted in the mapping either by writing them in the mapping literal, or with the indexing operator.

There is no specific order between the index-value pairs in a mapping, so there is no difference between the following two mapping literals:

```pike
([ 1:2, 3:4 ])
([ 3:4, 1:2 ])
```

If you try to look up an index that hasn't been inserted in the mapping, the indexing operator will return **0**:

```pike
english2spanish["cat"]     // Gives "gato"
english2spanish["glurble"] // Gives 0
```

Lookups are done using `==`, so the thing used as index in the lookup must be the **same** as the thing used when inserting things in the mapping. Remember that arrays, mappings and multisets may look the same, without being the same. Look at this example:

```pike
mapping(array(int) : int) m = ([ ]);
array(int) a = ({ 1, 2 });
m[a] = 3;
```

After running this code snippet, the expression `m[a]` will give the value **3**, but the expression `m[ ({ 1, 2 }) ]` will give the value **0**.

Mappings are similar to arrays. If you had a mapping from integers (to something), and used the integer values **0**, **1**, **2**, and so on, in order, this mapping would work very much like an array. But mappings are much more flexible, since you can use any type of values as indices. They are also slower and take up more space in the computer's memory.

Here are some useful things that you can do with mappings:

*   **Check if it is a mapping**  
    `mappingp(something)`
    
    The function mappingp returns **1** if the value _something_ is a mapping, otherwise **0**.
    

*   **Comparing mappings**  
    
    `mapping1 == mapping2` returns **1** if _mapping1_ and _mapping2_ are the same mapping, otherwise **0**. Just as with arrays, they have to be the _same_ mapping, not just equal. You can also use the operator `!=`, which means "not same". The relational operators (`<`, `>`, etc) do not work with mappings.
    

*   **Comparing mappings (again)**  
    
    `equal(mapping1, mapping2)` returns **1** if _mapping1_ and _mapping2_ look the same, otherwise **0**.
    

*   **Getting just the indices**  
    
    `indices(mapping)` returns an array containing all the indices from the index-value pairs in the mapping _mapping_.
    

*   **Getting just the values**  
    
    `values(mapping)` returns an array containing all the values from the index-value pairs in the mapping _mapping_. If you retrieve the indices (with indices) and the values (with values) from the same mapping, without performing any other mapping operations in between, the returned arrays will be in the same order. They can be be used as arguments to mkmapping to create an equivalent copy of the mapping.
    

*   **Create a mapping**  
    
    `mkmapping(index-array, value-array)` builds a new mapping with indices from the array _index-array_, and the corresponding values from the array _value-array_.
    

*   **Union**  
    `mapping1 | mapping2`
    
    You can use **set operations** such as **union** (|) on mappings. All the indices in a mapping are considered as a set, and the set operators work with these sets. The values just "tag along".
    
    The union operator returns a new mapping with the elements that are present in _mapping1_, or in _mapping2_, or in both. If an index is present in both mappings, the value part of the resulting index-value pair will come from the right-hand mapping (_mapping2_). Example:
    
    `([ 1:2, 3:4 ]) | ([ 3:5, 6:7 ])` gives the result `([ 1:2, 3:5, 6:7 ])`.
    
    Note that the elements in a mapping don't have a specified order.
    
    The **addition operator** (+) is a synonym for **union** (|) on mappings.
    

*   **Intersection**  
    
    `mapping1 & mapping2` returns a new mapping with the elements that are present in both mappings. The value parts of the resulting index-value pairs will come from the right-hand mapping (_mapping2_).
    
    `([ 1:2, 3:4 ]) & ([ 3:5, 6:7 ])` gives the result `([ 3:5 ])`.
    

*   **Difference**  
    
    `mapping1 - mapping2` returns a new mapping with the elements in the mapping _mapping1_ that are not also present in the mapping _mapping2_.
    
    `([ 1:2, 3:4 ]) - ([ 3:5, 6:7 ])` gives the result `([ 1:2 ])`.
    

*   **Exclusive or**  
    
    `mapping1_ ^ _mapping2` returns a new mapping with the elements that are present in _mapping1_ or in _mapping2_, but not in both.
    
    `([ 1:2, 3:4 ]) ^ ([ 3:5, 6:7 ])` gives the result `([ 1:2, 6:7 ])`.
    

*   **Finding the size**  
    
    `sizeof(mapping)` returns the number of index-value pairs in the mapping _mapping_.
    
    `sizeof( ([ ]) )` gives the result 0.
    

*   **Finding a value in a mapping**  
    `search(haystack, needle)`
    
    This is a "reverse lookup" that searches among the values of the index-value pairs instead of among the indices. It returns the index of the index-value pair that has the value _needle_ in the mapping _haystack_. If there are several index-value pairs that have the same _needle_ as value, any of them can be chosen. The comparison is done with `==`, so the element must be the same as the _needle_. Example:
    
    `search(([ 1:2, 3:4, 4:5, 7:4 ]), 4)` gives either **3** or **7**.
    

*   **Replacing values in a mapping**  
    
    `replace(mapping, old, new)` replaces all the values that are equal (with `==`) to _old_ with _new_. This operation does not create a copy, but changes the mapping _mapping_ itself. Example:
    
    `replace(([ 1:2, 2:3, 3:2 ]), 2, 17)` gives the result `([ 1:17, 2:3, 3:17 ])`.
    

*   **Checking if an index is present**  
    
    `zero_type(mapping[index])` returns **0** if the index _index_ is present in the mapping _mapping_, otherwise it returns something other than **0**. This can be useful to discriminate between an index that isn't present in the mapping, and one that is present but associated with the value **0**:
    
```pike
    if(temp["sauna"] == 0)
    {
      if(zero_type(temp["sauna"]))
        write("We don't know the temp in the sauna.\\n");
      else
        write("It's mighty cold in that sauna.\\n");
    }
```

The Data Type multiset
----------------------

A set is something where a value is either a member or not. A **multiset** (sometimes called a "bag") is a set where a value can be a member several times. The multiset can contain several copies of the same value.

A multiset literal can be written as a comma-separated list of the elements, inside `(< >)` like this:

```pike
(< "foo", "bar", "fum", "foo", "foo" >)
```

The data type of a set with elements of the type _element-type_ is written multiset(_element-type_). The data type `multiset(mixed:mixed)`, i. e. a multiset that can contain any types of elements, can also be written just multiset.

Here are a few variables that can contain multisets:

```pike
multiset(string) m;
multiset(int) mi = (< 1, -19, 0 >);
multiset(string) dogs = (< "Fido", "Buster" >);
```

A multiset variable that hasn't been given a value contains **0**, and not an empty multiset. If you want an empty multiset, you have to give it explicitly:

```pike
multiset(string) m1;  // m1 contains 0
m1 = (< >);  // Now m1 contains an empty multiset
multiset(int) m2 = (< >);
             // m2 contains an empty multiset
```

Multisets are very similar to **mappings**, except that:

*   Multisets just have elements, not index-value pairs.    

*   You can have multiple instances of the same element in a multiset, while in a mapping you can only have the same index in one single index-value pair.

Multisets also use the **indexing operator**, to see if an element is present, and to add and remove elements:

```pike
if(dogs["Fido"])
  write("Fido is one of my dogs.\\n");
if(!dogs["Dirk"])
  write("Dirk is not one of my dogs.\\n");
dogs["Kicker"] = 1; // Add Kicker to the set
dogs["Buster"] = 0; // Remove Buster
```

As you can see, you can write

`multiset[element] = 1`

to add the element _element_ to the multiset _multiset_, and

`multiset[element] = 0`

to remove it. Your program may be easier to understand if you use set operations instead:

```pike
dogs |= (< "Kicker" >); // Add Kicker to the set
dogs -= (< "Buster" >); // Remove Buster
```

There is no specific order between the index-value pairs in a multiset, so there is no difference between the following two multiset literals:

```pike
(< "foo", "bar" >)
(< "bar", "foo" >)
```

Here are some useful things that you can do with multisets:

*   **Check if it is a multiset**  
    `multisetp(something)`
    
    The function `multisetp` returns **1** if the value _something_ is a multiset, otherwise **0**.
    

*   **Comparing multisets**  
    
    `multiset1 == multiset2` returns **1** if _multiset1_ and _multiset2_ are the same multiset, otherwise **0**. Just as with arrays, they have to be the _same_ multiset, not just equal. You can also use the operator `!=`, which means "not same". The relational operators (`<`, `>`, etc) do not work with multisets.
    

*   **Comparing multisets (again)**  
    
    `equal(multiset1, multiset2)` returns **1** if _multiset1_ and _multiset2_ look the same, otherwise **0**.
    

*   **Set operations**  
    
    All the set operations work with multisets: **union** (|), **difference** (\-), **intersection** (&), and **exclusive or** (^).
    
