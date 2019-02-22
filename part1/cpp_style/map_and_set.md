## Set and Map

### Put object in to set/map
Need to define the < operator:
1) Define the '<' operator in the class:
```c++
struct Myobject {
	int val;
	int pos;
	// asending order
	bool operator < (const Myobject& obj) const {
		return val < obj.val;
	}
}
```

2) Define a functor as argument of the set
```c++
struct Comparator {
	bool operator() (const Myobject& obj1, const Myobject& obj2) const {
		return obj1.val < obj2.val;
	}
}

set<Myobject,Comparator> mySet;
```



---
### Find element in BST
lower_bound(x): iterator to first element >= x
upper_bound(x): iterator to first element > x
if the element do not exist, return end()

#### Find first element >= x
```c++
auto it = map.lower_bound(x);
if(it==map.end()) cout<<"Not Exist";
```

#### Find first element > x
```c++
// assume x is the integer
auto it = map.upper_bound(x);
if(it==map.end()) cout<<"Not Exist";
```

#### Find last element <= x
```c++
auto it = map.upper_bound(x);
if(it==map.begin()) cout<<"Not Exist";
--it; 
```

#### Find last element < x
```c++
auto it = map.lower_bound(x);
if(it==map.begin()) cout<<"Not Exist";
--it; 
```