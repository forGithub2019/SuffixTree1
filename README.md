# SuffixTree

Longest common substring problem 
[link](https://en.wikipedia.org/wiki/Longest_common_substring_problem)

Generalized suffix tree（广义后缀树 可以用来求解最长公共子串问题）
[link](https://en.wikipedia.org/wiki/Generalized_suffix_tree)

本文主要参考[link](https://www.geeksforgeeks.org/generalized-suffix-tree-1/)  （GeeksforGeeks 不错的网站， 详细介绍广义后缀树，包括实现）

在学习广义后缀树（Generalized suffix tree）之前，首先学习Ukkonen’s Suffix Tree Construction这个人的后缀树构建。

Ukkonen’s Suffix Tree Construction – Part 1：
[https://www.geeksforgeeks.org/ukkonens-suffix-tree-construction-part-1/](https://www.geeksforgeeks.org/ukkonens-suffix-tree-construction-part-1/)
    知识点包括：

1.  Implicit suffix tree（隐式后缀树，由于字符串S结尾不是唯一的字符$，某些后缀成为了其他后缀的前缀，这样这些后缀没有显式地体现在后缀树中，而是被某条其他后缀的边包含着）如下图

2.  High Level Ukkonen’s algorithm （for construct suffix tree withO(m) part部分介绍的算法还是O(m^3)，后面会不断简化， naïve algorithm need O(m^2）
Construct tree T1
For i from 1 to m-1 do
begin {phase i+1}

        For j <span <span class="hljs-keyword">class</span>=<span class="hljs-string">"hljs-keyword"</span>><span class="hljs-keyword">from</span></span> <span <span class="hljs-keyword">class</span>=<span class="hljs-string">"hljs-number"</span>><span class="hljs-number">1</span></span> <span <span class="hljs-keyword">class</span>=<span class="hljs-string">"hljs-keyword"</span>>to</span> i+<span <span class="hljs-keyword">class</span>=<span class="hljs-string">"hljs-number"</span>><span class="hljs-number">1</span></span>
          begin {extension j}
          Find <span <span class="hljs-keyword">class</span>=<span class="hljs-string">"hljs-keyword"</span>>the</span> <span <span class="hljs-keyword">class</span>=<span class="hljs-string">"hljs-keyword"</span>>end</span> <span <span class="hljs-keyword">class</span>=<span class="hljs-string">"hljs-keyword"</span>>of</span> <span <span class="hljs-keyword">class</span>=<span class="hljs-string">"hljs-keyword"</span>>the</span> path <span <span class="hljs-keyword">class</span>=<span class="hljs-string">"hljs-keyword"</span>><span class="hljs-keyword">from</span></span> <span <span class="hljs-keyword">class</span>=<span class="hljs-string">"hljs-keyword"</span>>the</span> root labelled S[j..i] <span <span class="hljs-keyword">class</span>=<span class="hljs-string">"hljs-keyword"</span>><span class="hljs-keyword">in</span></span> <span <span class="hljs-keyword">class</span>=<span class="hljs-string">"hljs-keyword"</span>>the</span> current tree.
          Extend <span <span class="hljs-keyword">class</span>=<span class="hljs-string">"hljs-keyword"</span>>that</span> path <span <span class="hljs-keyword">class</span>=<span class="hljs-string">"hljs-keyword"</span>>by</span> adding <span <span class="hljs-keyword">class</span>=<span class="hljs-string">"hljs-property"</span>>character</span> S[i+l] <span <span class="hljs-keyword">class</span>=<span class="hljs-string">"hljs-keyword"</span>><span class="hljs-keyword">if</span></span> <span <span class="hljs-keyword">class</span>=<span class="hljs-string">"hljs-keyword"</span>>it</span> <span <span class="hljs-keyword">class</span>=<span class="hljs-string">"hljs-keyword"</span>><span class="hljs-keyword">is</span></span> <span <span class="hljs-keyword">class</span>=<span class="hljs-string">"hljs-keyword"</span>>not</span> there already

&lt;span class="hljs-keyword"&gt;end&lt;/span&gt;;

end;

Suffix extension is all about adding the next character into the suffix tree built so far.
In extension j of phase i+1, algorithm finds the end of S[j..i] (which is already in the tree due to previous phase i) and then it extends S[j..i] to be sure the suffix S[j..i+1] is in the tree.

There are 3 extension rules:
Rule 1: If the path from the root labelled S[j..i] ends at leaf edge (i.e. S[i] is last character on leaf edge) then character S[i+1] is just added to the end of the label on that leaf edge.

Rule 2: If the path from the root labelled S[j..i] ends at non-leaf edge (i.e. there are more characters after S[i] on path) and next character is not s[i+1], then a new leaf edge with label s{i+1] and number j is created starting from character S[i+1].
A new internal node will also be created if s[1..i] ends inside (in-between) a non-leaf edge.

Rule 3: If the path from the root labelled S[j..i] ends at non-leaf edge (i.e. there are more characters after S[i] on path) and next character is s[i+1] (already in tree), do nothing.

One important point to note here is that from a given node (root or internal), there will be one and only one edge starting from one character. There will not be more than one edges going out of any node, starting with same character.

Ukkonen’s Suffix Tree Construction – Part 2：
知识点包括：

1.  Suffix links:
For an internal node v with path-label xA, where x denotes a single character and A denotes a (possibly empty) substring, if there is another node s(v) with path-label A, then a pointer from v to s(v) is called a suffix link.
If A is empty string, suffix link from internal node will go to root node.
There will not be any suffix link from root node (As it’s not considered as internal node).
2.  Skip/count trick1
Using suffix link along with skip/count trick, suffix tree can be built in O(m2) as there are m phases and each phase takes O(m).

3.  Trick 2
Stop the processing of any phase as soon as rule 3 applies. All further extensions are already present in tree implicitly.（所谓rule3 就是phase i进行到extension j时，如果没在labell j上增加character S[i]（即S[i]已经在labell j的尾部了），则phase i的后续j+1到m每个extension都不需要再增加character了也即，只要有extension j 用到rule3，本phase i就可以结束了。）

4.  Trick3
In any phase i, leaf edges may look like (p, i), (q, i), (r, i), …. where p, q, r are starting position of different edges and i is end position of all. Then in phase i+1, these leaf edges will look like (p, i+1), (q, i+1), (r, i+1),…. This way, in each phase, end position has to be incremented in all leaf edges. For this, we need to traverse through all leaf edges and increment end position for them. To do same thing in constant time, maintain a global index e and e will be equal to phase number. So now leaf edges will look like (p, e), (q, e), (r, e).. In any phase, just increment e and extension on all leaf edges will be done. Figure 19 shows this.

5.  So using suffix links and tricks 1, 2 and 3, a suffix tree can be built in linear time.（O(m)）

Ukkonen’s Suffix Tree Construction – Part 3：
Here we will take string S = “abcabxabcd” as an example and go through all the things step by step and create the tree.

We will state few more facts (which may be a repeat, but we want to make sure it’s clear to you at this point) here based on discussion so far:
    Phase 1 starts with Rule 2, all other phases start with Rule 1
    Any phase ends with either Rule 2 or Rule 3
    Any phase i may go through a series of j extensions (1 &lt;= j &lt;= i). In these j extensions, first p (0 &lt;= p &lt; i) extensions will follow Rule 1, next q (0 &lt;= q &lt;= i-p) extensions will follow Rule 2 and next r (0&lt;= r &lt;= 1) extensions will follow Rule 3. The order in which Rule 1, Rule 2 and Rule 3 apply, is never intermixed in a phase. They apply in order of their number (if at all applied), i.e. in a phase, Rule 1 applies 1st, then Rule 2 and then Rule 3
    In a phase i, p + q + r &lt;= i
    At the end of any phase i, there will be p+q leaf edges and next phase i+1 will go through Rule 1 for first p+q extensions
Active point:
        activePoint change for extension rule 3 (APCFER3): When rule 3 applies in any phase i, then before we move on to next phase i+1, we increment activeLength by 1. There is no change in activeNode and activeEdge. Why? Because in case of rule 3, the current character from string S is matched on the same path represented by current activePoint, so for next activePoint, activeNode and activeEdge remain the same, only activeLenth is increased by 1 (because of matched character in current phase). This new activePoint (same node, same edge and incremented length) will be used in phase i+1.

activePoint change for walk down (APCFWD): activePoint may change at the end of an extension based on extension rule applied. activePoint may also change during the extension when we do walk down. Let’s consider an activePoint is (A, s, 11) in the above activePoint example figure. If this is the activePoint at the start of some extension, then while walk down from activeNode A, other internal nodes will be seen. Anytime if we encounter an internal node while walk down, that node will become activeNode (it will change activeEdge and activeLenght as appropriate so that new activePoint represents the same point as earlier). In this walk down, below is the sequence of changes in activePoint:
(A, s, 11) — &gt;&gt;&gt; (B, w, 7) —- &gt;&gt;&gt; (C, a, 3)
All above three activePoints refer to same point ‘c’
Let’s take another example.
If activePoint is (D, a, 11) at the start of an extension, then while walk down, below is the sequence of changes in activePoint:
(D, a, 10) — &gt;&gt;&gt; (E, d, 7) — &gt;&gt;&gt; (F, f, 5) — &gt;&gt; (G, j, 1)
All above activePoints refer to same point ‘k’.
If activePoints are (A, s, 3), (A, t, 5), (B, w, 1), (D, a, 2) etc when no internal node comes in the way while walk down, then there will be no change in activePoint for APCFWD.
The idea is that, at any time, the closest internal node from the point, where we want to reach, should be the activePoint. Why? This will minimize the length of traversal in the next extension.

activePoint change for Active Length ZERO (APCFALZ): Let’s consider an activePoint (A, s, 0) in the above activePoint example figure. And let’s say current character being processed from string S is ‘x’ (or any other character). At the start of extension, when activeLength is ZERO, activeEdge is set to the current character being processed, i.e. ‘x             ’, because there is no walk down needed here (as activeLength is ZERO) and so next character we look for is current character being processed.

Ukkonen’s Suffix Tree Construction – Part 4,5：
举例进行了字符串”abcabxabcd”后缀树的构建。
构建过程中（part5中）我发现，suffix link的建立原则是，每个phase的最后一个新建的internal node指向root，而每个phase过程中构建的node会指向其后（同一个phase中的next extension）构建的internal node。如下图

图中node A B应该是phase 6构建的，suffix link是A-&gt;B, B-&gt;root，而phase 10中构建了C D E三个点，所以suffix link: C-&gt;D, D-&gt;E, E-&gt;root。上图中蓝色部分是part5中一段话，与我理解无太大偏差，但强调了一下实现过程中每次新建节点直接指向root的小技巧。
    Part5中根据phase 10指出几点注意事项：
We see following facts in Phase 10:

    Internal Nodes connected via suffix links have exactly same tree below them, e.g. In above Figure 40, A and B have same tree below them, similarly C, D and E have same tree below them.
    Due to above fact, in any extension, when current activeNode is derived via suffix link from previous extension’s activeNode, then exactly same extension logic apply in current extension as previous extension. (In Phase 10, same extension logic is applied in extensions 7, 8 and 9)
    If a new internal node gets created in extension j of any phase i, then this newly created internal node will get it’s suffix link set by the end of next extension j+1 of same phase i. e.g. node C got created in extension 7 of phase 10 (Figure 37) and it got it’s suffix link set to node D in extension 8 of same phase 10 (Figure 38). Similarly node D got created in extension 8 of phase 10 (Figure 38) and it got its suffix link set to node E in extension 9 of same phase 10 (Figure 39). Similarly node E got created in extension 9 of phase 10 (Figure 39) and it got its suffix link set to root in extension 10 of same phase 10 (Figure 40).
    Based on above fact, every internal node will have a suffix link to some other internal node or root. Root is not an internal node and it will not have suffix link.