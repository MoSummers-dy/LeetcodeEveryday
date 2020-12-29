---
description: '#sorting #heap'
---

# 692 Top K Frequent Words

Written: 2020-12-28

Given a non-empty list of words, return the k most frequent elements.

Your answer should be sorted by frequency from highest to lowest. If two words have the same frequency, then the word with the lower alphabetical order comes first.

## Method 1: Sorting

```java
public List<String> topKFrequent(String[] words, int k) {
        // Count the frequency
        Map<String, Integer> freqs = new HashMap<>();
        for (String word : words) {
            if (!freqs.containsKey(word)) {
                freqs.put(word, 0);
            }
            freqs.put(word, freqs.get(word) + 1);
        }
        
        List<String> counts = new ArrayList(freqs.keySet());
        
        // Sorting
        Collections.sort(counts, (w1, w2) -> {
            if (freqs.get(w1) != freqs.get(w2)) {
                return freqs.get(w2) - freqs.get(w1);
            }
            return w1.compareTo(w2);
        });
        
        return counts.subList(0, k);
    }
```

Time complexity: O\(nlogn\), naive sort is o\(nlogn\)  
Space complexity: O\(n\), for map and list

## Method 2: Heap

```java
public List<String> topKFrequent(String[] words, int k) {
        // Count the frequency
        Map<String, Integer> freqs = new HashMap<>();
        for (String word : words) {
            if (!freqs.containsKey(word)) {
                freqs.put(word, 0);
            }
            freqs.put(word, freqs.get(word) + 1);
        }
        
        // Set up comparator
        PriorityQueue<String> pq = new PriorityQueue<>((w1, w2) -> {
                if (freqs.get(w1) != freqs.get(w2)) {
                    return freqs.get(w1) - freqs.get(w2);
                }
                return w2.compareTo(w1);
            }
        );
        
        // Add to heap one by one
        for (String word : freqs.keySet()) {
            pq.add(word);
            if (pq.size() > k) {
                // since this is the min heap
                pq.poll();
            }
        }
        
        List<String> output = new ArrayList<>();
        while (!pq.isEmpty()) {
        // because this is a min heap, need to reverse
            output.add(0, pq.poll());
        }
        return output;
    }
```

Time Complexity: O\(nlogK\), logK time for each word  
Space Complexity: O\(K\), since the largest number of words in our minheap is K

