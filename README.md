# PS4-6.72-Explained
### TRIGGER FUNCTION
<p>
The trigger function exploits a type confusion vulnerability in the WebKit browser engine. This vulnerability allows accessing memory outside the bounds of an object by creating a type confusion between two objects.

In the trigger function, an object o is created with a property named a, which is type-confused with m_impl. A for...in loop is then created to iterate over the properties of the o object. Within the loop, a function i is hoisted onto the outer scope and overwrites the iteration variable i with the function i. This may invalidate the corresponding ForInContext object, which is a data structure used by JavaScriptCore to keep track of the state of for...in loops.

Next, the o object is accessed using the overwritten i variable. This triggers the op_get_direct_pname handler, which uses the property variable directly as a string object without any check. This can allow passing an arbitrary object as the property variable to the handler, resulting in a memory leak.

Finally, the function checks whether the type confusion was successful by comparing the impl.a property to the original target. If the comparison is successful, the function proceeds to leak object addresses, overwrite object addresses, and print leaked addresses. If the type confusion was successful and the exploit completed successfully, the function throws an error to signal success.
</p>
