# Hash Table (Python)

A hash table built from scratch in Python, without relying on Python's built-in `dict` hashing — instead, it implements its own simple hashing function and manages storage, insertion, deletion, and lookup manually.

## What Is a Hash Table?

A hash table is a data structure that stores **key-value pairs**. Instead of searching through every entry to find something (like you would with a list), a hash table converts each key into a number (a **hash**) and uses that number to decide exactly where the value should be stored. This makes adding, removing, and looking up values very fast on average, since you can jump almost directly to the right spot instead of scanning everything.

## The Hashing Function

For this project, the hashing function is intentionally simple: it takes a key (a string) and sums up the Unicode/ASCII numeric value of every character in it, using Python's built-in `ord` function. Two keys that contain the exact same characters — even in a different order — will produce the same hash value, since addition doesn't care about order. This is a deliberate design tradeoff for the lab: it keeps the hashing logic easy to understand, at the cost of being more collision-prone than a real production hash function.

## Handling Collisions

Because different keys can produce the same hash value (a **collision**), each hash value doesn't map directly to a single value — it maps to a small nested dictionary instead. That nested dictionary holds the *actual* key alongside its value. This approach is a form of **chaining**, a standard collision-resolution strategy: rather than overwriting data when two keys collide, both key-value pairs are kept safely side-by-side under the same hash "bucket," distinguished by their real key.

## Core Operations

The hash table supports four operations, mirroring what a real-world hash table (and Python's own `dict`) needs to support:

- **Hashing a key** — converts any string key into a numeric hash value.
- **Adding a key-value pair** — computes the key's hash, finds (or creates) the appropriate bucket, and stores the pair inside it.
- **Removing a key-value pair** — computes the key's hash, checks whether that key genuinely exists in the corresponding bucket, and deletes it if so. If the key was never stored, nothing happens and no error occurs.
- **Looking up a value by key** — computes the key's hash, checks whether the key exists in the corresponding bucket, and returns its value if found. If the key doesn't exist, it returns nothing rather than raising an error.

Every operation follows the same first step — hash the key — before deciding what to do with the result. This keeps the logic consistent across all four methods.

## Safety Around Missing Keys

A recurring theme throughout the implementation is **checking existence before acting**. Before removing or looking up a value, the code first confirms that the hash bucket exists, and only then checks whether the specific key exists inside it. This two-step check prevents errors that would otherwise occur from trying to access a bucket that was never created, and ensures the hash table behaves predictably even when asked about keys it has never seen.

## Python Concepts Used

- **Classes and instance methods** — the hash table's behavior and internal state are encapsulated inside a single class, with all interaction happening through its methods rather than direct access to its internals.
- **Dictionaries (including nested dictionaries)** — the core underlying storage mechanism, used both for the outer structure (hash value → bucket) and the inner structure (real key → value).
- **The `ord` function** — used to convert individual characters into their numeric Unicode/ASCII values, which is the basis of the whole hashing function.
- **Membership testing (`in`)** — used repeatedly to safely check whether a hash value or key exists before trying to access or modify it.
- **Short-circuit evaluation (`and`)** — combining two existence checks in a single condition, relying on Python not evaluating the second check if the first one already fails.

## Example Usage

```
ht = HashTable()
ht.add("cat", 1)
ht.add("act", 2)   # collides with "cat" — same hash, different key

ht.lookup("cat")   # returns 1
ht.lookup("dog")   # returns None — never added

ht.remove("cat")
ht.lookup("cat")   # returns None — successfully removed
```

## Possible Improvements

- Use a better hashing algorithm to reduce how often unrelated keys collide (e.g. incorporating character position, not just character value).
- Add an `update` or `contains` method for convenience.
- Add a method to return all stored keys or all stored values across every bucket.
