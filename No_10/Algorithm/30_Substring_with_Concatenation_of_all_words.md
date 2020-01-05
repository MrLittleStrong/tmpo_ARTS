## [30. Substring with Concatenation of All Words](https://leetcode-cn.com/problems/substring-with-concatenation-of-all-words/)

###Introduction

You are given a string, s, and a list of words, words, that are all of the same length. Find all starting indices of substring(s) in s that is a concatenation of each word in words exactly once and without any intervening characters.

**Example 1:**

```
Input:
  s = "barfoothefoobarman",
  words = ["foo","bar"]
Output: [0,9]
Explanation: Substrings starting at index 0 and 9 are "barfoor" and "foobar" respectively.
The output order does not matter, returning [9,0] is fine too.
```

**Example 2:**

```
Input:
  s = "wordgoodgoodgoodbestword",
  words = ["word","good","best","word"]
Output: []
```

###Solution

Consider to use a map to save count of words. In each round, we slice the source string and  compare with this map. 

###Code

```python
class Solution:
    def findSubstring(self, s: str, words: List[str]) -> List[int]:
        if len(words) == 0 or len(s) == 0: return []
        wordsDic = {}
        for word in words:
            if word in wordsDic.keys():
                wordsDic[word]+=1
            else:
                wordsDic[word]=1
        res = []
        wordLen = len(words[0])
        for i in range(len(s)-wordLen*len(words)+1):
            temp_wordsDic = wordsDic.copy()
            j = 0
            while(j<len(words)):
                temp_words = s[i+j*wordLen:i+(j+1)*wordLen]
                if temp_words in temp_wordsDic.keys():
                    temp_wordsDic[temp_words]-=1
                    if temp_wordsDic[temp_words]==0:
                        temp_wordsDic.pop(temp_words)
                else:
                    break
                j+=1
            if(j==len(words)):
                res.append(i)
        return res
```

###Performance

1. Time Complexity: 

   O(n)

2. Space Complexity:

   O(m)

