## Tries
```javascript
function Trie(letter = '') {
  this.value = letter;
  this.children = {};
  this.isWord = false;
}

Trie.prototype.add = function(word, node = this) {
  for(const letter of word) {
    if (!node.children[letter]) {
      node.children[letter] = new Trie(letter);
    }
    node = node.children[letter];
  }
  node.isWord = true;
  return this;
}

Trie.prototype.find = function(word, node = this) {
  for(const letter of word) {
    if (node.children[letter]) {
      node = node.children[letter];
    } else {
      return false;
    }
  }
  return (node.isWord);
}


const t2 = new Trie();
t2.add('బిరుదు').add('బిర్రు')
t2.find('బిర్రు')
```
