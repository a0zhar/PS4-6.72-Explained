# PS4-6.72-Explained
### TRIGGER FUNCTION
<p>
The function takes advantage of a type confusion vulnerability in the WebKit browser engine. The vulnerability allows to access memory outside the bounds of an object by creating a type confusion between two objects.<br>

In the ```trigger``` function, we first create an object ```o``` with a property named a which is type-confused with ```m_impl```. <br>
We then creates a for...in loop to iterate over the properties of the ```o``` object. <br>
Within the loop, we hoists a function ```i``` onto the outer scope and overwrites the iteration variable ```i``` with the function ```i```. <br>
This can result in an invalidation of the corresponding ForInContext object, which is a data structure used by JavaScriptCore to keep track of the state of for...in loops.

Next, we access the ```o``` object using the overwritten ```i``` variable. <br>
This results in the ```op_get_direct_pname``` handler being called, which uses the property variable directly as a string object without any check. <br>
This can allow us to pass an arbitrary object as the property variable to the handler, which can result in a memory leak.<br>

Finally, the function checks if the type confusion was successful by comparing the ```impl.a``` property to the original target.<br>
If the comparison is successful, the function proceeds to leak object addresses, overwrite object addresses, and print leaked addresses. <br>
If the type confusion was successful and the exploit completed successfully, the function throws an error to signal success.<br>
</p>
