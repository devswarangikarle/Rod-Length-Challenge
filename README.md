# Rod-Length-Challenge

As Maya embarked on her home renovation journey, she came across a long, straight iron rod marked from 0 to N. Eager to create the perfect space, she called in a skilled builder who would make K precise cuts along the rod. After each cut, Maya wanted to know the length of the longest remaining piece of the rod. For every cut made at position pᵢ, she needed to determine the maximum length of the pieces left after the i-th cut.
Can you help Maya figure out the longest piece of the rod after each cut?


def rod_length_challenge(N, K, cuts):
    import heapq

    # Store cut positions and segment boundaries
    cuts_positions = set([0, N])
    max_heap = [-(N)]  # Use negative values for max-heap

    # Map to track segment lengths and counts
    segment_counts = {N: 1}
    
    result = []
    
    for cut in cuts:
        # Find neighbors of the cut
        cuts_positions.add(cut)
        sorted_cuts = sorted(cuts_positions)
        idx = sorted_cuts.index(cut)
        left_cut = sorted_cuts[idx - 1]
        right_cut = sorted_cuts[idx + 1]
        
        # Update segment lengths
        current_length = right_cut - left_cut
        left_segment = cut - left_cut
        right_segment = right_cut - cut
        
        # Remove the old segment
        segment_counts[current_length] -= 1
        if segment_counts[current_length] == 0:
            del segment_counts[current_length]
        max_heap.remove(-current_length)
        heapq.heapify(max_heap)
        
        # Add new segments
        segment_counts[left_segment] = segment_counts.get(left_segment, 0) + 1
        segment_counts[right_segment] = segment_counts.get(right_segment, 0) + 1
        heapq.heappush(max_heap, -left_segment)
        heapq.heappush(max_heap, -right_segment)
        
        # Append the largest segment
        result.append(-max_heap[0])
    
    return result


# Input handling
N, K = map(int, input().split())
cuts = list(map(int, input().split()))

# Process and output results
result = rod_length_challenge(N, K, cuts)
print(" ".join(map(str, result)))
