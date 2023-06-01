+++

title = "Literate programming: Monte Carlo Tree Search"
description = "Redoing my part of a class project in Julia "
date = 2022-01-11
draft = true
tags = ["project", "literateprogramming", "python", "montecarlotreesearch"]

+++

## Literate programming: Monte Carlo tree search

### Backstory

There's a required class in my master's program called [Data and Visual Analytics](https://poloclub.github.io/#cse6242). It's infamous for the breadth of the material covered and the large number of tools used, including Python, JavaScript (mostly D3), SQL, Hadoop, Spark, Azure, AWS, and Google Cloud. Half of your course grade comes from the group project, where the required result was a web app showcasing analysis of a large dataset.

Each member of our project group came up with a proposal to vote on. The winner was a DOTA 2 hero recommendation system. If you don't know anything about DOTA 2, you're in the same boat I was.

After a reasonably thorough literature review (yes, there is DOTA 2 scientific literature), we decided to split our analysis into two tracks. One, a hero-similarity measure that would help the user find heroes they would enjoy trying, and two, a draft helper so that the user could choose an optimal team.

We had huge ambitions for our web app, but the trouble started early on. For the draft helper, one teammate and I were supposed to use Monte Carlo tree search (MCTS) simulation with a win condition based on a model that, given two teams of five heroes, predicts win-rate (based on Chen et al. REFERENCE HERE). Our other teammates were working on the web app and hero-similarity model.

Anyways, this teammate ended up dropping the course, so the draft helper ended up falling mostly to me. I spent a couple weekends early on getting the tree search working to my satisfaction. I was proud of myself; coming from mostly coding single scripts or notebooks, for this project I used multiple code files, classes, decorators, and type hints. Each function was carefully documented and my code appeared to spit out appropriate choices of heroes.

Then, up until the last weekend before the due date, I was just trying different models for win-rate prediction. That part didn't go well, but that's another story.

In reality, the tree search code wasn't doing anything close to what it was supposed to do. The following quote from [this post](https://jiby.tech/post/gherkin-features-user-requirements/) by Jb Doyon perfectly describes what happened:

> **The most common mistake I see developers make is building the wrong thing**: Not tracking exactly what is required from the start, and instead getting excited about the cool technical problems ahead. Fast forward a few days, weeks, months, showcasing the product near completion, suddenly the rift between what’s needed and what was built becomes obvious, in a frenzy of last minute changes to reach the original goal under pressure from deadlines.

I must have spent 40 hours over that 3-day weekend getting everything working, with the help of one very patient teammate. It helped so much to have someone to write to about the bugs I was seeing. Putting my thoughts into written words helped clarify the problem, which was a subtle but fundamental flaw in my implementation. We ended up getting an A, but this has been bugging me ever since.

So that's how I learned that I should use tests, logging, and probably some other stuff I've never even heard of. Inspired by [Literate programming wordle](https://jiby.tech/project/literate_wordle/wordle.html) by Jb Doyon, which I saw on [Hacker News](https://news.ycombinator.com/item?id=31306340), I want to try to code this up again and see if I can do better this time.

### Monte Carlo Tree Search: a quick explanation







### Application to the Dota 2 draft



Everything you need to know about Dota 2 for this post to make sense:

1. There are two teams of five players each.
2. The players must each select from a pool of 123 possible heroes in a draft at the beginning of the match.
3. There are several variations of draft rules, which will become important later.
4. Then they go fight each other until one team wins.



(Chen et al. REFERENCE HERE) noted that the Dota 2 draft can be modeled as a two-player game, with each player representing a team (Radiant or Dire). There are various game modes that change the draft rules; we'll focus on Ranked All Pick and Captain's Mode. 

Ranked All Pick has three rounds: for the first two, each team picks two heroes concurrently before that rounds' picks are revealed. In the third round, teams pick a single, final hero. If teams choose the same hero during a round, that hero is banned and the round restarts. While MCTS can be adapted to simultaneous play \cite{l2013convergence} and imperfect knowledge situations, we feel the added complexity is unnecessary for this application and will model the All Pick draft as sequential with perfect information. Captain's Mode, where a captain chooses heroes to ban as well as play, is already sequential with perfect information.

When modeled as a tree, the Dota 2 draft has a branching factor at each depth of $121-d$, where $\{d\,|\,[0\;\ldotp \ldotp\;10]\}$ is the tree depth of the tree. MCTS is an anytime algorithm: it can be stopped early and still return useful results. If left to run, it will converge on the minimax solution. The algorithm~\cite{mctsorig} works in a loop of $\textit{selection}$, $\textit{expansion}$, $\textit{simulation}$, and $\textit{backpropagation}$. During the selection phase, the algorithm traverses the tree according to the tree policy. Nodes represent a game state (the heroes picked thus far) and edges represent hero picks. Game states are represented by a 121-length vector, where each index maps to a hero and $\texttt{1}$, $\texttt{-1}$, $\texttt{0}$ are a Radiant pick, Dire pick, and unpicked, respectively. 



ANIMATION HERE (PROBABLY MAKE DURING/AFTER CODING)



A common tree policy function, used in both \cite{chen2018art, chen2021heroes}, is Upper Confidence Bounds (UCB) 1 (called Upper Confidence applied to Trees (UCT) when combined with MCTS)~\cite{kocsis_bandit_2006}): 

$$\begin{equation}
  \mathrm{argmax}\left\{\frac{w_i}{s_i} + c \sqrt{\frac{\ln{s_p}}{s_i}}\right\}
\end{equation}$$

where $w_i$ is the child node win count, $s_i$ is the child node simulation count, $c$ is an empirically-determined constant, and $s_p$ is the simulation count of the parent node. The purpose of UCB1 is to balance exploitation and exploration; the left term favors successful nodes, while the right term encourages new exploration. 

Once the tree policy leads to an unvisited node, the algorithm enters the expansion phase. If the unvisited node is not terminal, a new child node is created. During simulation, a draft is played out until the end by the rollout policy (random selection). A winner is decided, in our case, by inference from a binary classification model that takes the final game state as input and predicts the winner. Finally the expanded node and its ancestors' statistics are updated during backpropagation.

This process is repeated until the stopping criterion is reached. Our planned changes to the base UCT algorithm are in the selection phase, and include:

- Limiting the search space at different depths by allowing players to specify draft strategies based on hero roles identified in our analysis.
- Experimenting with different node statistics, potentially tracking a reward variable instead of simple win count.
- Experimenting with custom tree policies. \cite{silver_mastering_2017, ye2021mastering} add a prior probability term for each node, which limits the exploration range to more useful choices.
- Adding the ban component for Captain's Mode.
