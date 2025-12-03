Analysis of GitHub Commit Counting in Pull Requests
A. Factual Behavior Confirmation

The factual behavior of GitHub's contribution graph is that commits made on a branch that is part of an open Pull Request (PR) do not typically count toward the daily contribution graph until the PR is merged into the default branch (usually main or master).

Contribution Determination: GitHub's contribution graph primarily determines a contribution by activities that result in a permanent, shared record on a repository's default branch. This includes:

Commits made to the default branch (or the selected branch for contributions).

Merging a Pull Request (the commits are then attributed to the dates they were originally made).

Opening an issue or a Pull Request.

Proposing a Pull Request review.

Commits in PR Branches Before Merge: Commits on a feature branch used for a PR are not counted immediately on the daily graph. They are in a state of limbo until they are incorporated into the default branch via a merge or a squash.

Immediate Contributions: Activities that contribute immediately to the daily count include:

Opening a new Issue.

Opening a new Pull Request.

Submitting a Pull Request Review comment or approval.

Committing directly to the default branch.

B. Design Rationale

GitHub's decision to only count commits upon merge, or to count issues/PRs upon creation, is likely an intentional design choice rooted in fundamental version control and professional metrics philosophies.

Preventing Misuse of Activity Metrics: By deferring the commit count until the merge, GitHub prevents developers from artificially inflating their daily contribution metrics through frequent, small commits on isolated feature branches that may ultimately be discarded, squashed, or abandoned. The merge acts as a gatekeeper verifying the work is complete and accepted.

Aligning with Traditional Version-Control Philosophies: In traditional workflows, a "contribution" is often viewed as a change that has become a permanent, accepted part of the project's canonical history. A commit on a feature branch is, by definition, tentative until it is integrated. Counting only upon merge aligns the contribution graph with the concept of a finalized, integrated codebase change.

Maintaining System Simplicity: A simpler system tracks integration, not just creation. It avoids complex logic to assess the "completeness" or "value" of an unmerged commit, and it prevents the need to retroactively remove contributions if a feature branch is deleted without merging.

C. Analysis of Modern Concerns

This legacy approach can clash with contemporary software development practices, leading to a perception that the contribution graph doesn't fully reflect a developer's real-time effort.

Contemporary Workflow Differences: Modern development often favors long-lived feature branches and extensive review cycles. Developers may work intensely on a PR for several days, submitting multiple commits, but the contribution graph shows no commit activity until the final merge. This makes the contribution history appear sporadic rather than continuous.

Impact on Contribution Visibility: The daily effort spent coding and committing is invisible until the work is integrated. This obscures the consistent, day-to-day coding discipline of a developer, especially in teams with stringent code review processes that naturally delay merges.

Perception of Developer Activity: This behavior can lead to an inaccurate perception of a developer's activity or productivity. A developer spending a week making daily commits and refining a feature appears "inactive" on the graph, while a developer who merges five PRs in a single day appears highly productive, regardless of when the work was actually done.

Interaction with Modern Collaboration Patterns: Modern patterns like trunk-based development (TBD) and short-lived branches mitigate this issue, but the friction remains for teams relying on longer feature branches and complex PRs that necessitate back-and-forth collaboration within the PR branch itself.

D. Proposed Improvement Options

1. Expanding Activity on PRs

This involves counting commits on a branch as soon as the Pull Request is opened.

Advantages: Increases real-time visibility of effort; aligns the graph with the start of the public review process.

Drawbacks: Potential for misuse via commits to unverified, unshared branches; requires complex logic to handle force-pushes or branch deletions.

2. Introducing Conditional Criteria

This involves counting commits only if a minimum threshold of collaborator reviews/comments is reached on the PR.

Advantages: Offers flexibility to only count "high-value" activity; encourages and recognizes collaboration metrics.

Drawbacks: Adds significant complexity to the contribution tracking engine; metrics could be gamed by requiring frivolous reviews/comments.

3. Recognizing Collaborative Signals

This involves explicitly counting all PR interaction activities (commenting, requesting changes, review, etc.) as distinct, weighted contributions.

Advantages: Accurately reflects non-coding, high-value work; validates the effort of technical leads and reviewers.

Drawbacks: Overemphasis on administrative tasks vs. direct coding; increases the noise-to-signal ratio in the contribution graph.

4. Providing Additional Visualizations

This involves introducing a secondary "Activity Tracker" that shows real-time unmerged commit activity, separate from the main contribution graph.

Advantages: Keeps the main graph simple while offering deeper insight; preserves the legacy definition of a "contribution" (merged code).

Drawbacks: Requires users to navigate to a secondary view for full context; may confuse users about which visualization is the "official" metric.

E. Concluding Recommendation

The most balanced solution is to Preserve the Contribution Graph's focus on Integrated Code while Introducing a Separate Activity Tracker Visualization.

Justification: This approach respects both the legacy consideration and the modern workflow. The main contribution graph continues to serve its original, important purpose: to reflect a developer's contribution to the permanent, accepted history of the codebase, counting only changes that have passed through the quality gate (the merge). This preserves the metric's integrity. The separate, opt-in "Activity Tracker" (or a similar supplementary visualization) directly addresses the need for real-time visibility by showing unmerged commits and collaborative signals. This offers comprehensive data without compromising the definition of the canonical contribution metric.