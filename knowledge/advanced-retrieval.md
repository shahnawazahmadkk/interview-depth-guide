# Approximate Data Structures and Vector Retrieval

Use this material when the problem needs it, usually after foundations. Technology trivia is not a substitute for a coherent design. Product names below identify implementations, not endorsements or required choices.

## Approximation by query type

| Need | Structure | Guarantee and limitation |
|---|---|---|
| Set membership | Bloom filter | Negative is definitive for correctly inserted items; positives can be false |
| Frequency of a known item | Count-Min Sketch | Overestimates with nonnegative updates; does not enumerate keys |
| Number of distinct items | HyperLogLog | Approximate cardinality; no member list or exact count |
| Distribution/percentiles | Histogram or quantile sketch | Precision depends on buckets or algorithm; merging rules matter |

### Bloom filter

Allocate m bits and use k hashes to set positions for each item. Query the same positions. No unset bit means only possible membership. Concurrent updates, incomplete replication, or an unsynchronized filter can break application-level assumptions even if the abstract structure has no false negatives.

For expected n items and false-positive target p, approximate sizing is `m = -n * ln(p) / (ln(2)^2)` bits and `k = (m/n) * ln(2)`. At one billion items and 1% false positives, this is about 9.59 billion bits, or 1.20 GB in decimal units. State units rather than rounding away a large allocation.

Standard filters do not support deletion. Rebuild, rotate filters, or deliberately choose a deletion-capable variant when needed. For an exhaustive crawler, use a positive result to trigger an authoritative visited-set check; dropping all positive URLs sacrifices completeness. For database/cache guards, define how every writer keeps the filter current.

### Count-Min Sketch

Use d rows with independent hashes into w counters. Increment one counter per row for each nonnegative update; query the minimum. Collisions inflate counts. Width and depth control additive-error and confidence bounds relative to total update mass, not a guaranteed relative error for each rare key.

For approximate top-K, keep an explicit candidate structure in addition to the sketch. The sketch cannot reconstruct item IDs. Evaluate candidate admission and ranking errors; this is not automatically an exact top-K algorithm. Time windows require an expiry/rotation strategy, not unlimited accumulation.

### HyperLogLog

Hash distinct values uniformly, use some bits to choose a register, and record the maximum observed leading-zero rank in the remaining bits. The estimator combines transformed register values with bias/range corrections; it is not simply the harmonic mean of raw zero counts. Error scales approximately as `1.04 / sqrt(m)` for m registers under the usual assumptions.

Suitable for approximate unique users, distinct keys, or URLs. Merge compatible sketches register-wise with maxima for set union; do not sum daily unique counts to obtain monthly uniques. Standard HLL does not directly support deleting an item or exact intersection. Database availability varies; PostgreSQL commonly uses an extension rather than a built-in HLL type.

### Histograms and quantiles

Count values in explicit ranges. Sum counts until the requested rank is reached and report the containing bucket or an explicitly approximate interpolation. Choose bounds around the actual SLO; exponential buckets help cover broad ranges. Merge compatible histograms before computing fleet percentiles; averaging per-host p99 values is invalid.

If counts are 1,000 in 0-10 ms, 8,000 in 10-50 ms, 800 in 50-100 ms, 180 in 100-500 ms, and 20 above 500 ms, there are 10,000 samples. Rank 9,500 lies in 50-100 ms, so p95 is in that bucket. Cumulative buckets count a sample in every containing upper-bound bucket. Metric-label cardinality also needs limits.

## Vectors and similarity

An embedding is a numeric representation produced by a model for a particular notion of similarity. Store the model/version, dimension, normalization, and object identifier. Query and stored vectors must belong to compatible spaces. Higher dimension does not guarantee better product relevance.

Cosine similarity compares direction; dot product also depends on magnitude unless vectors are normalized; Euclidean distance measures geometric separation. Hamming distance counts differing bits for binary codes. Choose the metric supported by the model's training objective. For dot-product similarity, larger is generally more similar; do not accidentally minimize it as a distance.

Exact search compares against every eligible vector. Its cost includes dimension and candidate selection, approximately `O(n*d + n*log(k))` with a bounded heap. SIMD/GPU batching can make exact search worthwhile for a small filtered set or strict recall requirement. ANN trades recall for latency, memory, and build/update cost. Recall@K measures recovered exact neighbors, not user-perceived relevance.

## Index choices

| Family | Mechanism | Main trade-off |
|---|---|---|
| HNSW | Multi-layer neighbor graph; sparse upper navigation and broader base search | Often strong empirical recall/latency; graph memory and update/build cost |
| IVF | Assign vectors to centroids, probe selected clusters | Probe count trades work for recall; distribution drift can require retraining |
| LSH | Similarity-preserving hashes create candidate buckets | Tables consume memory; more hash bits per table reduce collisions and may reduce recall |
| Random projection forest | Search multiple trees split by geometric planes | Useful for static, memory-mapped indexes; some implementations require rebuilds for changes |

Do not promise logarithmic worst-case HNSW search, universal 95% recall, or a fixed 2x memory multiplier. Dimension, connectivity, precision, dataset, and tuning matter. Memory mapping avoids eager deserialization but not page faults or disk latency during cold queries.

## Filtering and hybrid retrieval

Filter before search when the eligible set is small enough for an exact scan. Post-filtering an ANN candidate set can underfill results; oversampling helps but does not guarantee K results. Integrated filtering and iterative search are implementation-specific and may stop at resource limits. In pgvector, iterative scans can continue through the index after filtering, subject to configured limits; verify the installed version. See the [pgvector reference](https://github.com/pgvector/pgvector).

Enforce tenant and permission boundaries before exposing results or sending context to a model. Combine lexical retrieval and semantic retrieval when both exact terminology and meaning matter. Merge with an explicit rank-fusion or calibrated scoring scheme, not by adding unrelated raw scores without justification.

For two-stage retrieval, retrieve a broader candidate set and rerank it with a costlier model. Evaluate recall before reranking, final relevance, p95/p99 latency, and resource cost with representative queries and filter selectivities.

## Architecture and evolution

An existing database extension may be sufficient. A separate vector service is useful when workload isolation, indexing features, or measured scale warrant it. A purpose-built vector index does not replace transactional business storage, although a transactional database with vector support can serve both roles.

Typical path: ingest source -> normalize/chunk -> embed -> index -> retrieve eligible IDs -> fetch current source records -> optionally rerank or generate an answer. Track document changes, deletions, model versions, and indexing lag. For RAG, retrieved material is data, not instructions; preserve authorization, return supporting evidence, and evaluate answer grounding separately from ANN recall.

For high write rates, a searchable recent-data buffer plus a background-built index is one option, not a universal database design. Soft deletions require exclusion during queries and eventual compaction. A model migration may need parallel indexes and dual ingestion until a measured cutover; vectors from different model spaces cannot simply be mixed.

Useful scenarios: semantic document/code search, recommendations, similar media, duplicate detection, and knowledge retrieval. A fraud/anomaly use case requires a defined scoring model; semantic distance alone is not proof of fraud.
