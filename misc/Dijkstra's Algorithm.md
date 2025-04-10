Finds the shortest path between nodes in a weighted graph. Works by maintaining a set of unvisited nodes and continuously updating their distances as shorter paths are found.

```typescript
type WeightedNode = {
  node: string;
  weight: number;
};

function dijkstra(
  graph: Map<string, WeightedNode[]>,
  start: string
): Map<string, number> {
  const distances = new Map<string, number>();
  const visited = new Set<string>();
  const pq = new PriorityQueue<[string, number]>(); // node, distance pairs

  // Initialize distances
  for (const node of graph.keys()) {
    distances.set(node, Infinity);
  }
  distances.set(start, 0);
  pq.enqueue([start, 0], 0);

  while (!pq.isEmpty()) {
    const [current, currentDistance] = pq.dequeue()!;

    if (visited.has(current)) continue;
    visited.add(current);

    // For each neighbor of current node
    for (const { node: neighbor, weight } of graph.get(current) || []) {
      if (visited.has(neighbor)) continue;

      const distance = currentDistance + weight;

      // If we found a shorter path, update it
      if (distance < distances.get(neighbor)!) {
        distances.set(neighbor, distance);
        pq.enqueue([neighbor, distance], distance);
      }
    }
  }

  return distances;
}

// Example usage:
const graph = new Map([
  [
    "A",
    [
      { node: "B", weight: 4 },
      { node: "C", weight: 2 }
    ]
  ],
  ["B", [{ node: "E", weight: 3 }]],
  [
    "C",
    [
      { node: "D", weight: 2 },
      { node: "E", weight: 1 }
    ]
  ],
  ["D", [{ node: "E", weight: 3 }]],
  ["E", []]
]);

const shortestDistances = dijkstra(graph, "A");
// Returns: Map(5) { 'A' => 0, 'B' => 4, 'C' => 2, 'D' => 4, 'E' => 3 }
```

Key points about Dijkstra's:

- Only works with positive weights
- Uses a priority queue for efficiency
- Finds shortest path to ALL nodes from start node
- Can be modified to track the actual path by storing predecessors
