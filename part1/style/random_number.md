## Random Number
***
The code to use the nextInt() method to generate an integer within a range is:  
```java
public static int generateRandomIntIntRange(int min, int max) {
	Random r = new Random();
	return r.nextInt((max - min) + 1) + min;
}
```

#### Shuffle
Shuffle cards  
```java
public void shuffle(int[] cards) {
	Random r = new Random();
	int len = cards.length;
	//swap random card from card[0,...51] with card[51], then 
	//swap random card from card[0,...50] with card[50], then 
	for(int i=len; i>=1; i--) {
		int rand = r.nextInt(52); //generate random num in [0,...,51]
		swap(card,rand,i);
	}
}
```

Randomly select k number from an array
```java

```

#### Reservoir Sampling  
```java
// call this function to add new sample and return a random sample
// 0, 1, 2, 3, 4, 5
int totalSample = 0;
int result = 0;
public int ReservoirSampling(int sample) {
	totalSample ++;
	int r = random(1,totalSample);//random int from [1,totalSample]
	if(r==1) result = sample;
	return result;
}
```
Extend: Randomly select k numbers
```java
int totalSample = 0;
int result[] = new int[k];
"Initialize all the k samples for the first k input"
public int[] ReservoirSampling(int sample) {
	totalSample ++;
	int r = random(1,totalSample);//random int from [1,totalSample]
	if(r<=k) result[r-1] = sample;
	return result;
}
```