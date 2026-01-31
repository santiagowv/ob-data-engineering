Unified model for defining both <span style="color:rgb(216, 203, 251)">batch and streaming data-parallel processing</span> pipelines.
# Execution model
The elements in a `PCollection` are <span style="color:rgb(216, 203, 251)">processed in bundles.</span>
- The division of the collection into bundles is <span style="color:rgb(216, 203, 251)">arbitrary and selected by the runner</span>.
- Allows the runner to choose between <span style="color:rgb(216, 203, 251)">persisting results after every element</span>, and having to <span style="color:rgb(216, 203, 251)">retry everything if there is a failure</span>.
- A streaming runner may prefer to <span style="color:rgb(216, 203, 251)">process and commit small bundles</span>.
- A batch runner may prefer to <span style="color:rgb(216, 203, 251)">process larger bundles</span>.
# Failures and parallelism
## Data-parallelism within one transform
When executing a single `ParDo`, <span style="color:rgb(216, 203, 251)">a runner might divide an example input of nine elements collection into two bundles</span>.
![[Pasted image 20260103113548.png|500]]
## Dependent-parallelism between transforms
`ParDo` transforms that are in sequence may be <span style="color:rgb(216, 203, 251)">dependently parallel</span>.
![[Pasted image 20260103113845.png|400]]
- The first worker executes `ParDo1` on the elements in bundle A (which results in bundle C), and then executes `ParDo2` on the elements in bundle C.
- The second worker executes `ParDo1` on the elements in bundle B (which results in bundle D), and then executes `ParDo2` on the elements in bundle D.
![[Pasted image 20260103114044.png|550]]
## Failures within one transform
If processing of an element within a bundle fails, <span style="color:rgb(216, 203, 251)">the entire bundle fails</span>. Elements in the bundle must be retried (otherwise <span style="color:rgb(216, 203, 251)">the entire pipeline fails</span>).
![[Pasted image 20260103114926.png|600]]
- The processing <span style="color:rgb(216, 203, 251)">completes successfully the second time</span>. The retry <span style="color:rgb(216, 203, 251)">does not necessarily happend on the same worker as the original processing attempt</span>.
## Failures between transforms
Because the runner was executing `ParDo1` and `ParDo2` together, the output bundle from `ParDo1` must also be thrown away, and <span style="color:rgb(216, 203, 251)">all elements in the input bundle must be retried</span>.
![[Pasted image 20260103115259.png|600]]