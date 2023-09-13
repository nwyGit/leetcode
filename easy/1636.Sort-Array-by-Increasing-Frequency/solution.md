# 1636. Sort Array by Increasing Frequency

### Solving idea

At first, I am thinking about recording the frequency in a dictionary type(key/value). For each unique value, it will store the frequency each time the data gets access. After traversing all the data, the total frequency of the data will be stored to the related key. Then, we will know the data with the highest frequency and insert it to the list. Finally we return the list.

## Attempt

```java
class Solution {
    public int[] frequencySort(int[] nums) {
        HashMap<Integer, Integer> map = new HashMap<>();
        ArrayList<Integer> list = new ArrayList<>();

        for (int i = 0; i < nums.length; i++) {
            if (map.containsKey(nums[i])) {
                map.replace(nums[i], map.get(nums[i]) + 1);
            } else {
                map.put(nums[i], 1);
            }
        }

        for (Map.Entry<Integer, Integer> entry : map.entrySet()) {
            list.add(entry.getValue());
        }

        Collections.sort(list);

        int[] result = new int[nums.length];

        int index = 0;
        for (int num : list) {
          int key = 0;
          for (Map.Entry<Integer, Integer> entry : map.entrySet()) {
                if (entry.getValue().equals(num)) {
                    key = entry.getKey();
                }
            }
          for (int i = 0; i < num; i++) {
              result[index++] = key;
          }
        }

        return result;
    }
}
```

## Solution

```java
class Solution {
	public int[] frequencySort(int[] nums) {
		Map<Integer, Integer> map = new HashMap<>();
		// count frequency of each number
		Arrays.stream(nums).forEach(n -> map.put(n, map.getOrDefault(n, 0) + 1));
		// custom sort
		return Arrays
			.stream(nums)
			.boxed()
			.sorted(
				(a,b) -> map.get(a) != map.get(b) ?
				map.get(a) - map.get(b) : b - a)
			.mapToInt(n -> n)
			.toArray();
	}
}
```

## Syntax explanation

Map<Integer, Integer> map = new HashMap<>();  
+ Store the data to the key and the frequency to the value.

Arrays.stream(nums)  
+ Stream takes reference type as argument and returns object Streams.  
+ Arrays.stream can take primitive type and reference type as argument and returns Specialized Primitive Streams(IntStream etc.) or object Streams.  
+ In this case, it takes int[](primitive type) and returns IntStream.  

.forEach(n -> map.put(n, map.getOrDefault(n, 0) + 1));  
+ "forEach" method is used to iterate over each element of the IntStream.  
+ For each element in the IntStream, it updates the frequency of the data or initialize it to 1.

Arrays.stream(nums)  
+ same as above.  

.boxed()  
+ use "boxed" method to convert it to object stream.

.sorted((a,b) -> map.get(a) != map.get(b) ? map.get(a) - map.get(b) : b - a)  
+ sort the stream with custom comparator.
+ if the frequencies are not equal, data with higher frequency goes first
+ if the frequencies are equal, larger data goes first

.mapToInt(n -> n)  
+ converts the object stream back to IntStream.

.toArray();  
+ converts the sorted IntStream to an integer array.