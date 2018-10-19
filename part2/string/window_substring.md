## Window Substring
---
This is the templete for solving the window substring problem using two pointers. The template is revised based on [this](https://leetcode.com/problems/minimum-window-substring/discuss/26808/Here-is-a-10-line-template-that-can-solve-most-'substring'-problems.com)

#### Template
```java
int findSubstring(string s){
    int[] tab = new int[128];
    int cnt; // check whether the substring is valid
    int i=0, j=0; //two pointers, one point to tail and one  head
    int d; //the length of substring

    for() { /* initialize the hash map here */ }

    for(j=0;j<s.length();j++){

        if(tab[s.charAt(j)]?){  
            cnt?/* modify counter here */ 
            tab[s.charAt(j)]++;
            j++;
        }

        while(/* counter condition */){ 
             
             /* update d here if finding minimum*/

            //increase begin to make it invalid/valid again  
            if(tab[s.charAt(i)]?){ 
                cnt?/*modify counter here*/ 
                tab[s.charAt(i)]--;
                i++; 
            }
        }  

        /* update d here if finding maximum*/
    }
    return d;
  }
```