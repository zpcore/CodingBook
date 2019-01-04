## Recusion Early Termination
***

How to terminate recursion as soon as we got the result?

```java
boolean stop = false; // use global varible

public T recursion() {
	if(stop==true || ...) return ... // check at base case

	recursion();
}
```