# Intuition

The problem asks us to find the number of groups of similar strings. We can form the groups by finding pairs of similar strings and merging them together. We can use a disjoint set data structure to keep track of the groups and merge them.
# Approach

Create a disjoint set data structure with n elements, where n is the length of the input array of strings.
Iterate over all possible pairs of strings in the array.
For each pair of strings, check if they are similar (differ in exactly two positions, or are identical).
If they are similar, merge the sets to which they belong using the disjoint set data structure.
After iterating over all pairs of strings, count the number of distinct sets in the disjoint set data structure and return it as the answer.
# Complexity
- Time complexity:
Time complexity: O(n^2 * m), where n is the length of the input array of strings and m is the length of the strings. This is because we compare every pair of strings, which takes O(m) time each, and we do this n(n-1)/2 times, resulting in a total time complexity of O(n^2 * m).

- Space complexity:
 O(n), where n is the length of the input array of strings. This is because we store the disjoint set data structure, which takes O(n) space.
<!-- Add your space complexity here, e.g. $$O(n)$$ -->

# Code
```
class disjointSet{
    int[] set;
    int[] rank;
    disjointSet(int n){
        set=new int[n];
        rank=new int[n];
        for(int i=0;i<n;i++){set[i]=i;}
        
    }
    int find(int x){
        if(x==set[x]){return x;}
        return find(set[x]);
    }
    void union(int x, int y) {
        int parentX = find(x);
        int parentY = find(y);
        if (parentX == parentY) {
            return;
        }
        if (rank[parentX] < rank[parentY]) {
            set[parentX] = parentY;
        } else {
            set[parentY] = parentX;
            if (rank[parentX] == rank[parentY]) {
                rank[parentX]++;
            }
        }
    }

}
class Solution {
    private boolean isPair(String a, String b) {
        if(a.equals(b)){return true;}
        int diffCount = 0;
        for(int i=0;i<a.length();i++) {
            if(a.charAt(i)!=b.charAt(i)) {
                diffCount++;
                if(diffCount>2){return false;}
            }
        }
        return diffCount == 2;
    }

    public int numSimilarGroups(String[] strs) {
        //no of possible pairs = n(n-1)/2    
        disjointSet djs=new disjointSet(strs.length);
        for(int i=0;i<strs.length-1;i++){
            for(int j=i+1;j<strs.length;j++){
                if(isPair(strs[i],strs[j])){
          //          System.out.println(strs[i]+" "+strs[j]);
                    djs.union(i,j);
                }
            }
        }
        //System.out.println(Arrays.toString(djs.set));
        HashSet<Integer> parent=new HashSet<>();
        for(int i:djs.set){
            parent.add(djs.find(i));
        }
        //System.out.println(parent);
        return parent.size();
    }
}
```
