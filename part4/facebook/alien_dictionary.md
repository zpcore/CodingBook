## Alien Dictionary (Leetcode 269 mutation) (Correctness Unchecked)
Simplify version of Leetcode 269
---

#### Question
Given String[] words and char[] ordering，decide whether the words match with the ordering，return true or false.  
E.g. words = {"apple", "append", "boy", "zoo", "zpcore"}, ordering = {'a','b','z','g','o','p'}, return true;  
words = {"apple", "append", "boy", "zoo", "zpcore"}, ordering = {'b','a','b','z','g','o','p'}, return false;  


#### Solution
KeyNote: 
1. Only compare the two first characters from the two string where the mismatch starts.
2. We can use hashmap to judge the two characters' sequence in O(1) time complexity. Map(character->position in ordering)

```java
public boolean orderMatch(String[] words, char[] ordering) {
	Map<Character,Integer> hm = new HashMap<>();
	for(int i=0;i<ordering.length;i++) hm.put(ordering[i],i);
	for(int i=1;i<words.length;i++) {
		String s1 = words[i-1];
		String s2 = words[i];
		int j=0;
		while(j<s1.length() && j<s2.length() && s1.charAt(j)==s2.charAt(j)) j++; //skip the prefix identical characters
		if(j<s1.length() && j<s2.length()) {
			if(hm.get(s1.charAt(j)>hm.get(s2.charAt(j)))) return false;
		}
	}
	return true;
}
```