# Implement Trie (Prefix Tree)

A trie(sometimes called a prefix tree) is a variant of an n-ary tree in whcih characters are stored at each node. Each path down the tree may represent a word. At each end of the path, a * node is used to indicate complete words. 

Very commonly, a trie is used to store the entire English language for quick prefix lookups. While a hast table can quickly lookup whether a string is a valid word, it cannot tell us if a string is a prefix of any valid words. A trie can do this very quickly. 

Also, you can use trie to do url indexing for a web crawler to check if you have visit this url before. Another greate way to do this is using [bloom filter](http://billmill.org/bloomfilter-tutorial/).


Here is a implementation of prefix tree by using Java:

```java
class TrieNode {
    // Initialize your data structure here.
    public char label;
    public boolean leaf = false;
    public TrieNode[] children = new TrieNode[26];

    public TrieNode() {}
    public TrieNode(char label) {
        this.label = label;
    }
}

public class Trie {
    private TrieNode root;

    public Trie() {
        root = new TrieNode();
    }

    TrieNode node;

    public int find (String word) {
        int i, size = word.length();

        for (i = 0; i < size; i++) {
            if (node.children[word.charAt(i) - 'a'] == null) break;
            else node = node.children[word.charAt(i) - 'a'];
        }

        return i;
    }

    // Inserts a word into the trie.
    public void insert(String word) {
        node = root;
        int i, size = word.length();

        for (i = find(word); i < size; i++) {
            node.children[word.charAt(i) - 'a'] = new TrieNode(word.charAt(i));
            node = node.children[word.charAt(i) - 'a'];
        }

        node.leaf = true;
    }

    // Returns if the word is in the trie.
    public boolean search(String word) {
        node = root;

        if (find(word) == word.length() && node.leaf == true) return true;
        else return false;
    }

    // Returns if there is any word in the trie
    // that starts with the given prefix.
    public boolean startsWith(String prefix) {
        node = root;

        if (find(prefix) == prefix.length()) return true;
        else return false;
    }
}


```