# Top k frequent words
## https://leetcode.com/problems/top-k-frequent-words

Given an array of strings words and an integer k, return the k most frequent strings.

Return the answer sorted by the frequency from highest to lowest. Sort the words with the same frequency by their lexicographical order.

 
```
Example 1:

Input: words = ["i","love","leetcode","i","love","coding"], k = 2
Output: ["i","love"]
Explanation: "i" and "love" are the two most frequent words.
Note that "i" comes before "love" due to a lower alphabetical order.

Example 2:

Input: words = ["the","day","is","sunny","the","the","the","sunny","is","is"], k = 4
Output: ["the","is","sunny","day"]
Explanation: "the", "is", "sunny" and "day" are the four most frequent words, with the number of occurrence being 4, 3, 2 and 1 respectively.
``` 

## Constraints:

1. 1 <= words.length <= 500
2. 1 <= words[i] <= 10
3. words[i] consists of lowercase English letters.
4. k is in the range [1, The number of unique words[i]]

### Naive approach :
1. First create a map (Map<String,Integer>) for frequency (how many times individual word appear in the input aray).

2. Second step, reverse the map, so the frequency becomes the key and the value will be List<String> which appear that many times. (Use a TreeMap<Integer, List<String>>). Since we are intersted in the Top K words, using a TreeMap in second step will give us the access to words with most frequency.    

3. Last step is to take the k words from the TreeMap, since by default TreeMap sorts the keys in ascending order, we can use the lastEntry() method on reverse map.
   
#### Don't forget to remove the word from the reverse map after considering it in the top k frequent words output List.   
#### And to get the key and value from a Map.Entry, you can use getKey() and getValue() methods respectively.

## Implementation :
```java
class Solution {
    public List<String> topKFrequent(String[] words, int k) {
        Map<String,Integer> freq = new HashMap<>();
        for(String word : words) {
            int occurance = freq.getOrDefault(word, 0);
            freq.put(word, occurance + 1);
        }
        TreeMap<Integer,List<String>> freqToWords = new TreeMap<>();
        for(String key : freq.keySet()) {
            freqToWords.putIfAbsent(freq.get(key), new ArrayList<String>());
            freqToWords.get(freq.get(key)).add(key);
        }
        for(int key : freqToWords.keySet()) {
            Collections.sort(freqToWords.get(key));
        }
        List<String> result = new ArrayList<>();
        for(int i = 0; i < k; i++) {
            Map.Entry<Integer,List<String>> lastEntry = freqToWords.lastEntry();
            List<String> values = lastEntry.getValue();
            result.add(values.remove(0));
            if(values.size() == 0)
                freqToWords.remove(lastEntry.getKey());
        }
        return result;
    }
}
```

