# Lab Report 3
## Part 1 - Bugs
Failure inducing input:
```java
@Test
public void testFilter() {
    List<String> list = Arrays.asList("a", "b", "c", "d", "e");
    StringChecker sc = s -> s.compareTo("c") > 0;
    List<String> result = ListExamples.filter(list, sc);
    assertEquals(Arrays.asList("d", "e"), result);
}
```
Input that doesn't induce a failure:
```java
@Test
public void testFilter2() {
    List<String> list = Arrays.asList("a", "b", "c", "d");
    StringChecker sc = s -> s.compareTo("c") > 0;
    List<String> result = ListExamples.filter(list, sc);
    assertEquals(Arrays.asList("d"), result);
}
```
The symptom:

![first test failing](images/lab3/bug-symptom.png)

The bug:
```java
static List<String> filter(List<String> list, StringChecker sc) {
    List<String> result = new ArrayList<>();
    for(String s: list) {
        if(sc.checkString(s)) {
            result.add(0, s);
        }
    }
    return result;
}
```
The fixed code:
```java
static List<String> filter(List<String> list, StringChecker sc) {
    List<String> result = new ArrayList<>();
    for(String s: list) {
        if(sc.checkString(s)) {
            result.add(s);
        }
    }
    return result;
}
```
Changing `result.add(0, s)` to `result.add(s)` fixed the code because adding at the end preserves the order while adding at the beginning reverses it.
## Part 2 - Researching Commands
I chose the command `grep` and the options `-C`, `-v`, `-o`, `-e`. I learned all of these from [this blog](https://zwischenzugs.com/2022/02/02/grep-flags-the-good-stuff/).
### `-C [num]`
- Prints `num` leading and trailing lines of context for each match.
- This is useful to identify how the matched string is being used in the surrounding context.

Example 1
```bash
$ grep -C1 "base pair" technical/plos/*.txt
```
```
technical/plos/journal.pbio.0020190.txt-        DNA generally? Or does it happen to particular sequences? Bacteria have their chi (χ)
technical/plos/journal.pbio.0020190.txt:        sequence, which is a specific series of eight base pairs in the DNA of the bacterial
technical/plos/journal.pbio.0020190.txt-        chromosome that stimulate the action of proteins that bring about recombination (Eggleston
--
technical/plos/journal.pbio.0020190.txt-        occur than in other places on the chromosome. Recombination hotspots are local regions of
technical/plos/journal.pbio.0020190.txt:        chromosomes, on the order of one or two thousand base pairs of DNA (or less—their length is
technical/plos/journal.pbio.0020190.txt-        difficult to measure), in which recombination events tend to be concentrated. Often they
--
technical/plos/journal.pbio.0020223.txt-        with reagents that are covalently linked to complementary DNA oligonucleotides. Upon
technical/plos/journal.pbio.0020223.txt:        Watson-Crick base pairing, the proximity of the synthetic reactive groups elevates their
technical/plos/journal.pbio.0020223.txt-        effective molarity by several orders of magnitude, inducing a chemical reaction. Because
```
This gives context to the appearance of "base pair".

Example 2
```bash
$ grep -C 2 "type IIa" technical/biomed/*.txt
```
```
technical/biomed/1472-6793-2-8.txt-        classification scheme, fibers are characterized as follows:
technical/biomed/1472-6793-2-8.txt-        type I fibers are slow-twitch with a high oxidative
technical/biomed/1472-6793-2-8.txt:        capacity; type IIa fibers are fast-twitch with a high
technical/biomed/1472-6793-2-8.txt-        oxidative and glycolytic capacity; type IIb fibers are
technical/biomed/1472-6793-2-8.txt-        fast-twitch with a low oxidative and a high glycolytic
--
technical/biomed/rr196.txt-          diaphragm with monoclonal antibodies specific for the
technical/biomed/rr196.txt-          following MHCs: NOQ7.5.4D for type I [ 23 ] , SC-71 for
technical/biomed/rr196.txt:          type IIa [ 24 ] , and BF-F3 for type IIb [ 24 ] . The
technical/biomed/rr196.txt-          antibody useful for the identification of type IIx
technical/biomed/rr196.txt-          fibers, BF-35 [ 24 ] , stains all fibers except pure IIx
```
This shows why "type IIa" is mentioned.
### `-v`
- Inverts the match, matching all lines that do NOT contain the pattern.
- This is useful when the inverse pattern is easier to match.

Example 1
```bash
$ grep -v "," technical/plos/pmed.0020191.txt
```
```
permission? Who is to decide what is “historically significant”? Not to mention the
meta-question: who is to decide who is to decide? I apologize to the authors if my brief
comments [2] implied that they took a position on this issue.
```
This finds all lines without commas in them.

Example 2
```bash
$ grep -v "\." technical/plos/pmed.0020226.txt
```
```
Richard Smith's key suggestion [1] is that medical journals “should stop publishing
strive to better enforce their conflicts-of-interest disclosure rules, drug companies will
strive to find or create other publication outlets that can communicate to physicians
clinical trial reports from medical journals might be an even larger proliferation of frank
```
This finds all lines without periods in them.
### `-o`
- Outputs only the text matched and not the whole line
- This is useful if you only care about what is matched.

Example 1
```bash
$ grep -o "\S*ion" technical/plos/pmed.0020191.txt
```
```
investigation
question
institution
permission
mention
meta-question
position
```
This finds all words ending in -ion.

Example 2
```bash
$ grep -o "\bre\S*" technical/plos/pmed.0020226.txt
```
```
relevance
reports
```
This finds all words with the prefix re-.
### `-e [pattern]`
- Allows multiple patterns to be matched simultaneously.
- While there are other ways to achieve the same thing, this helps make patterns less complex.

Example 1
```bash
$ grep -e "\.$" -e ",$" technical/plos/pmed.0020191.txt
```
```
The excellent article by Jordan Paradise, Lori B. Andrews, and colleagues, “Ethics.
remains: if such guidelines were to be established, what individuals, institutions,
comments [2] implied that they took a position on this issue.
```
This matches lines ending in a period or comma.

Example 2
```bash
$ grep -e "\band\b" -e "\bor\b" technical/plos/pmed.0020226.txt
```
```
trials” and concentrate on “critically evaluating them.” This bold and radical suggestion
strive to find or create other publication outlets that can communicate to physicians
advertising outlets and messages that might more effectively catch doctors' attentions.
```
This matches all lines containing "and" or "or".