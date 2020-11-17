# Trie Problems

## Add and Search Word - DS Design
[Link](https://leetcode.com/problems/add-and-search-word-data-structure-design/)
```python
class TrieNode():
  def __init__(self):
    self.children = collections.defaultdict(TrieNode)
    self.isWord = False

class WordDictionary:
  def __init__(self):
    self.root = TrieNode()
  
  def addWord(word):
    node = self.root
    for letter in word:
      node = node.children[letter]
    node.isWord = True

  def search(word):
    node = self.root
    self.res = False
    self.dfs(node, word)
    return self.res

  def dfs(node, word):
    if not word:
      if node.isWord:
        self.res = True
      return
    
    if word[0] == ".":
      for n in node.children.values():
        self.dfs(n, word[1:])
    else:
      node = node.children.get(word[0])
      if not node:
        return
      self.dfs(node, word[1:])
```
Notes:
- A Trie is composed of TrieNode's
- A Trie has a root
- Adding a word to a Trie: Keep following the letter/child chain and mark isWord at the end
- DFS tip, slice your word one letter at a time `word[1:]`, check `if not word` as a stopping condition
- During a search through a trie, always be sure node is not None


## Design Search Autocomplete System
[Link](https://leetcode.com/problems/design-search-autocomplete-system/)
```python
class TrieNode():
  def __init__(self):
    self.children = {}
    self.isEnd = False
    self.data = None
    self.rank = 0


class AutocompleteSystem:
  def __init__(self, sentences, times):
    self.root = TrieNode()
    self.keyword = ""
    for i, sentence in enumerate(sentences):
      self._addRecord(sentence, times[i])


  def _addRecord(self, sentence, hot_degree):
    node = self.root
    for ch in sentence:
      if ch not in node.children:
        node.children[ch] = TrieNode()
      
      # progress down the trie
      node = node.children[ch]
    
    # reach the end
    node.isEnd = True
    node.data = sentence
    node.rank -= hot_degree # store negative for sorting


  def input(self, ch):
    results = []
    if ch != "#":
      self.keyword += c
      results = self._search(self.keyword)
    else:
      self._addRecord(self.keyword, 1)
      self.keyword = ""
    
    return [item[1] for item in sorted(results)[:3]]


  def _search(self, sentence):
    node = self.root
    for ch in sentence:
      if ch not in node.children:
        return []
      node = node.children[ch]
    return self._dfs(node)


  # this is searching past the sentence, this explores the trie
  def _dfs(self, root):
    ret = []
    if root:
      if root.isEnd:
        ret.append((root.rank, root.data))
      
      for child in root.children:
        ret.extend(self._dfs(root.children[child]))
    return ret
```

