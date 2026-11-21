<img src="assets/logo.png" width="88" height="88" alt="Blyx retrieval logo">

# Blyx Retrieval

Evaluate ranked document IDs for a search or retrieval-augmented generation system. If a retriever returns the same relevant document twice, the duplicate occupies a rank but earns no extra credit.

## Run

With Blyx 0.2.1, Rust 1.85 or newer, and a platform linker, run `blyxc native-run example.blyx` or `blyxc native-run tests.blyx`. To import this library in another Blyx project:

```sh
git clone --depth 1 --branch v0.1.1 https://github.com/Rahulchaube1/blyx-retrieval.git vendor/retrieval
```

```blyx
use vendor::retrieval::retrieval;
fn main() {
    let scores = retrieval_at_k([7, 7, 9], [7, 9], 3);
    println!(at(scores, 1));
}
```

Save the program beside `vendor` and run `blyxc native-run main.blyx`. It prints recall of 1.

## Score one query

`retrieval_at_k(ranked, relevant, k)` returns precision, recall, reciprocal rank, and average precision at the cutoff, in that order. Use the test set's original retrieval order. Binary judgments identify relevant documents; any other ID counts as a miss. Repeated judgments count once.

- Precision divides unique hits by k, including unfilled slots when a system returns fewer than k results.
- Recall divides unique hits by the number of distinct relevant documents.
- Reciprocal rank is the inverse of the first hit's one-based rank, or zero for no hits.
- Average precision sums the precision at each new hit and divides by the smaller of k and the number of distinct relevant documents.

Supply whole, nonnegative document IDs at most 9,007,199,254,740,991. An empty judgment set, a nonpositive k, or an invalid ID in the evaluated data returns an empty vector. An empty ranking with at least one judgment returns four zeros. Only the first k ranked items are examined. This implementation uses scans over small per-query vectors for duplicate detection. Average the per-query metrics over a documented set of judged queries to compare runs.

[Blyx package catalog](https://www.blyx-lang.space/packages) · [Blyx compiler](https://github.com/Rahulchaube1/blyxxxx/releases/tag/v0.2.1)

## Contribute

Run the documented tests, then open a pull request with a small input that reproduces the behavior you want to change. Include the expected result and your Blyx compiler version. [Package issues](https://github.com/Rahulchaube1/blyx-retrieval/issues) · [Ecosystem catalog](https://www.blyx-lang.space/packages)
