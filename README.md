import java.util.*;

class TrieNode {
    Map<Character, TrieNode> children = new HashMap<>();
    boolean isEndOfWord = false;
}

public class Trie {
    private TrieNode root;

    public Trie() {
        root = new TrieNode();
    }

    public void insert(String word) {
        TrieNode node = root;
        for (char c : word.toCharArray()) {
            node = node.children.computeIfAbsent(c, x -> new TrieNode());
        }
        node.isEndOfWord = true;
    }

    public boolean search(String word) {
        TrieNode node = root;
        for (char c : word.toCharArray()) {
            if (!node.children.containsKey(c)) return false;
            node = node.children.get(c);
        }
        return node.isEndOfWord;
    }

    public List<String> getAllWords() {
        List<String> res = new ArrayList<>();
        collectWords(root, "", res);
        return res;
    }

    public List<String> getWordsWithPrefix(String prefix) {
        List<String> res = new ArrayList<>();
        TrieNode node = root;
        for (char c : prefix.toCharArray()) {
            if (!node.children.containsKey(c)) return res;
            node = node.children.get(c);
        }
        collectWords(node, prefix, res);
        return res;
    }

    private void collectWords(TrieNode node, String prefix, List<String> res) {
        if (node.isEndOfWord) res.add(prefix);
        for (char c : node.children.keySet()) {
            collectWords(node.children.get(c), prefix + c, res);
        }
    }

    public static void main(String[] args) {
        Trie trie = new Trie();
        trie.insert("apple");
        trie.insert("app");
        trie.insert("bat");
        trie.insert("batman");
        trie.insert("batter");

        System.out.println("Search 'bat': " + trie.search("bat")); // true
        System.out.println("Search 'bath': " + trie.search("bath")); // false

        System.out.println("All words:");
        for (String word : trie.getAllWords()) {
            System.out.println(word);
        }

        System.out.println("Words with prefix 'bat':");
        for (String word : trie.getWordsWithPrefix("bat")) {
            System.out.println(word);
        }
    }
}
