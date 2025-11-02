/**
 * Merges overlapping or nearby time ranges based on a given threshold.
 * Each range is represented as [start, end], where end is exclusive.
 *
 * Example:
 * mergeTimeRanges([[0, 5], [5, 10], [12, 15]], 3)
 * → [[0, 15]]
 *
 * @param {Array<Array<number>>} ranges - Array of [start, end] pairs.
 * @param {number} threshold - Gap threshold in milliseconds.
 * @returns {Array<Array<number>>} - Array of merged, non-overlapping, sorted ranges.
 */

export function mergeTimeRanges(ranges, threshold = 0) {
  if (!Array.isArray(ranges) || ranges.length === 0) return [];

  // Step 1: Sort ranges by start time
  const sorted = ranges
    .map(r => {
      if (!Array.isArray(r) || r.length !== 2 || isNaN(r[0]) || isNaN(r[1])) {
        throw new Error("Invalid range format. Each range must be [start, end].");
      }
      return r;
    })
    .sort((a, b) => a[0] - b[0]);

  // Step 2: Merge logic
  const merged = [sorted[0]];

  for (let i = 1; i < sorted.length; i++) {
    const [currStart, currEnd] = sorted[i];
    const last = merged[merged.length - 1];
    const [lastStart, lastEnd] = last;

    // If ranges overlap or the gap ≤ threshold, merge them
    if (currStart <= lastEnd + threshold) {
      last[1] = Math.max(lastEnd, currEnd);
    } else {
      merged.push([currStart, currEnd]);
    }
  }

  return merged;
}

/** Example usage when run directly */
if (import.meta.url === `file://${process.argv[1]}`) {
  const example = [
    [0, 5],
    [5, 10],
    [12, 13],
    [14, 20],
    [25, 30]
  ];
  const threshold = 3;

  console.log("Input:", JSON.stringify(example));
  console.log("Threshold:", threshold);
  console.log("Merged Output:", JSON.stringify(mergeTimeRanges(example, threshold)));
}
