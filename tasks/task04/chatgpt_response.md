A. Factual Behavior of GitHub’s Contribution Graph
GitHub’s contribution graph only counts certain actions as contributions.
For commits to appear on the graph, they must satisfy conditions such as:
Being authored with an email linked to the user’s GitHub account. 
Being made in specific branches of a repository (typically the default branch, e.g., main/master, or gh-pages for project pages).
Commits made on branches used for Pull Requests do not count toward the contribution graph until those commits reach the default (or counted) branch, usually by being merged. 
Some non-commit actions (e.g., creating a repository or forking one) always count as contributions; opening a pull request can also qualify as a contribution event, subject to GitHub’s criteria. 
Answer to the core question:
Daily commits made inside a Pull Request on a non-default branch do not immediately appear as contributions on the graph.
This behavior is documented and intentional rather than an implementation bug. 
B. Design Rationale (Conceptual)
Several broad design motives can explain this behavior:
Metric Integrity
Limiting counted contributions to commits that reach key branches reduces opportunities to inflate activity with throwaway or experimental branches.
This helps keep the graph closer to a record of code that actually enters the primary codebase.
Alignment with Version-Control Philosophy
Traditional workflows treat integration into the main line as the meaningful milestone.
Counting only integrated commits reinforces the idea that lasting impact is measured where code is ultimately consumed.
System Simplicity and Predictability
Restricting counting rules to a small set of branches keeps the contribution logic easier to understand and maintain.
It avoids complex edge cases arising from numerous ephemeral branches and transient states of work in progress.
C. Modern Concerns
From a contemporary perspective, this behavior can raise several conceptual issues:
Mismatch with Current Workflows
Modern development is often highly branch- and PR-centric, with substantial effort occurring before integration.
A model that focuses only on integrated work may under-represent the process-oriented nature of present-day collaboration.
Limited Visibility of Ongoing Activity
When only integrated work is counted, periods of intense work on long-running branches may appear as inactivity.
This can reduce the clarity of a person’s or team’s day-to-day engagement when viewed through the graph alone.
Distorted Perception of Contribution Levels
Observers relying on the graph might infer lower or inconsistent activity compared to the actual effort being invested.
This can influence impressions of reliability, consistency, or engagement in ways that do not fully reflect reality.
Reduced Representation of Collaborative Dynamics
Modern collaboration emphasizes iterative review, refinement, and experimentation.
A model that records only final integration events may fail to capture the richness of these interactions and the value of intermediate contributions.
D. Improvement Options
Below are generalized redesign directions, framed at a conceptual level.
Option 1: Broaden What Counts as Activity
Idea:
Extend contribution counting to include a wider set of branch-based or PR-related events, not only those on specific branches.
Potential Advantages:
More accurately reflects the full range of work performed.
Provides a richer view of development effort across the lifecycle of changes.
Potential Drawbacks:
Increases the risk of metric inflation through trivial or low-value events.
May make the contribution graph noisier and harder to interpret.
Option 2: Introduce Conditional Criteria
Idea:
Count additional activities only when certain conditions are met (e.g., association with specific repository states, policies, or qualifiers).
Potential Advantages:
Preserves some quality control while acknowledging more forms of work.
Enables repositories or organizations to tune what they consider meaningful activity.
Potential Drawbacks:
Adds configuration complexity and potential inconsistency between repositories.
Makes the rules less transparent to casual observers.
Option 3: Recognize Collaborative Signals
Idea:
Incorporate non-commit signals—such as structured review interactions, approvals, or sustained engagement on a change—as recognized activity types.
Potential Advantages:
Reflects the collaborative nature of modern development more fully.
Acknowledges contributions that are not strictly code changes but still critical to progress.
Potential Drawbacks:
Requires careful definition of which interactions are meaningful enough to count.
Could be harder to summarize succinctly in a simple visual representation.
Option 4: Add Complementary Activity Visualizations
Idea:
Preserve the existing contribution graph as-is, but introduce additional visual views focused on pre-integration and PR-centric activity.
Potential Advantages:
Separates “finalized contributions” from “work in progress,” providing two distinct but complementary perspectives.
Avoids altering existing expectations while still surfacing richer information.
Potential Drawbacks:
Increases UI and cognitive complexity for users interpreting multiple views.
Requires users to learn which visualization to use for which purpose.
Option 5: Weighted or Categorized Contributions
Idea:
Classify activities into categories (e.g., integrated, in-progress, review) and optionally weight or display them differently.
Potential Advantages:
Offers a nuanced representation that distinguishes different contribution types.
Enables more sophisticated downstream interpretation by humans or tools.
Potential Drawbacks:
May reduce the intuitive simplicity of the existing single-color heatmap model.
Could encourage over-optimization around particular categories or weights.
E. Concluding Recommendation
A balanced approach would:
Retain the existing integration-focused graph as a representation of code that has reached key branches, preserving its historical intent and relative robustness against metric inflation.
Introduce a complementary visualization (or set of metrics) that captures branch- and PR-focused activity as a first-class concept, including ongoing, pre-integration work.
This combination:
Honors legacy design goals around simplicity and resistance to gaming.
Better aligns with modern, PR-driven workflows by surfacing the full lifecycle of work, not just its final state.
Provides observers with a more nuanced understanding of activity, without forcing a single metric to serve every interpretive need.
By clearly distinguishing between integrated contributions and ongoing work, such a design can offer transparency, fairness, and contemporary relevance while still remaining conceptually coherent and practically usable.