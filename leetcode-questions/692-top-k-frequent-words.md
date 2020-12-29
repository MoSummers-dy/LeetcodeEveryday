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

## Method 3: BucketSort + Trie

This code is from: [https://leetcode.com/problems/top-k-frequent-words/discuss/108399/Java-O\(n\)-solution-using-HashMap-BucketSort-and-Trie-22ms-Beat-81](https://leetcode.com/problems/top-k-frequent-words/discuss/108399/Java-O%28n%29-solution-using-HashMap-BucketSort-and-Trie-22ms-Beat-81)

```java
public List<String> topKFrequent(String[] words, int k) {
        // calculate frequency of each word
        Map<String, Integer> freqMap = new HashMap<>();
        for(String word : words) {
            freqMap.put(word, freqMap.getOrDefault(word, 0) + 1);
        }
        // build the buckets
        TrieNode[] count = new TrieNode[words.length + 1];
        for(String word : freqMap.keySet()) {
            int freq = freqMap.get(word);
            if(count[freq] == null) {
                count[freq] = new TrieNode();
            }
            addWord(count[freq], word);
        }
        // get k frequent words
        List<String> list = new LinkedList<>();
        for(int f = count.length - 1; f >= 1 && list.size() < k; f--) {
            if(count[f] == null) continue;
            getWords(count[f], list, k);
        }
        return list;
    }
    
    private void getWords(TrieNode node, List<String> list, int k) {
        if(node == null) return;
        if(node.word != null) {
            list.add(node.word);
        }
        if(list.size() == k) return;
        for(int i = 0; i < 26; i++) {
            if(node.next[i] != null) {
                getWords(node.next[i], list, k);
            }
        }
    }
    
    private boolean addWord(TrieNode root, String word) {
        TrieNode curr = root;
        for(char c : word.toCharArray()) {
            if(curr.next[c - 'a'] == null) {
                curr.next[c - 'a'] = new TrieNode();
            }
            curr = curr.next[c - 'a'];
        }
        curr.word = word;
        return true;
    }
    
    class TrieNode {
        TrieNode[] next;
        String word;
        TrieNode() {
            this.next = new TrieNode[26];
            this.word = null;
        }
    }
```

## Other Methods

You can read more here: [https://leetcode.com/problems/top-k-frequent-words/discuss/431008/Summary-of-all-the-methods-you-can-imagine-of-this-problem](https://leetcode.com/problems/top-k-frequent-words/discuss/431008/Summary-of-all-the-methods-you-can-imagine-of-this-problem)

