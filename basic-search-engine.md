# A Basic Search Engine

### Design a basic search database

---

# A First Approach

What if I told you that I want to search in a document?

You'll probably just use `ripgrep` or a similar tool to find a word in a bunch of documents.

Let's explore this solution a little bit!

---

# The Issues

 1. What if you have a lot of documents?

 1. What if you want to search for multiple words?

 1. What if your query words have typos?

 1. What if you want to search in specific fields and only display some?

---

# A lot of documents

Using `ripgrep` on a filesystem is `O(n)` where `n` is the number of characters in the set of documents you are searching in.

It means that the longer and the more your documents become the more time it will take.

We can design a `O(1)` algorithm where the time it takes to search in a set of documents is constant and doesn't depend on the size of it.

---

# A lot of documents

By preparing the dataset in advance we can create an inverted index where the key would be the word and the value the set of documents that contains the word.

We can extract the query words and look into the inverted index to retrieve the documents which contain our query.

A typical lookup into a `BTree` is `O(log(n))` where `n` is the number of entries i.e. `log(1000)` is 3, `log(1000000)` is 6.

---

# Multiple words

Reducing the dataset by calling `ripgrep` with the next word on the result of the current word can work but that is not ideal.

It will take a lot of time to process and all the results **must** contain all the query words.

Another solution could be to call `ripgrep` on the whole dataset with a _regex_ that defines each query word as optional i.e. `(hello|world|meili|search)`.

---

# Multiple words

Designing the previously described behavior with our inverted index is pretty simple.

We extract the query words as before and query the inverted index with every single word.

If we want to keep the documents containing **all** the query words we simply do an intersection, otherwise we do a union.

---

# Query words have typos

Unfortunately, `ripgrep` doesn't support typo-tolerancy. One way to do it would be to create a list of all the possible words that comes from query words and use them when searching.

Our search engine could have an optimized word dictionnary. It is possible to query this data-structure for all the words that matches certain patterns i.e. typos and prefixes.

The final step is to query the inverted-index with the resulting words and retrieve the associated documents.

---

# Structured documents

It can be quite complex to design a search engine that is simply using the file system to query documents without searching in some specific fields.

One solution could be to separate the searchable fields form the others.

Once you've done that you need to reconstruct the original documents by retrieving the part that must only be displayed elsewhere.

---

# Structured documents

In our search engine, it can be easy to onle extract the words from the searchable fields, associate the whole document with an _id_, and store this _id_ in the inverted index.

When the query is done on the word dictionnary only the searchable fields can match the query.

The last step is to retrieve the whole document that is stored in a key-value store using the document _id_ from the inverted index.

---

# Questions?

---

# Advanced questions

 1. What about a lot more documents?
 2. What is a key-value store and how can it be designed?
 3. Which performance issues can we have with this design?
 4. How can we improve the relevancy of the engine?
 5. Which kind of language can we support?
