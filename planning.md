# TakeMeter Planning

## 1. Community

I chose the ongoing One Piece manga/anime community because One Piece is my favorite anime. After starting watching One Piece I frequently started seeing One Piece posts across all of my social networks. On those posts I've seen several discussions about One Piece such as fans debating among themselves about a hot take, fans making theories what might happen later on in the series, talks about story events, fans reactions and opinion about the manga/anime and many more. This frequent discussions of the series makes it a good source for diverse discourse.

## 2. Label Taxonomy

### Fact

Posts that discuss confirmed events or information from the manga or anime.

Examples:

* Vegapunk died during the Egghead arc.
* Luffy defeated Kaido in Wano.

### Theory

Posts that speculate about future events or discuss unconfirmed ideas.

Examples:

* Joy Boy is still alive.
* Imu is related to Nefertari Lily.

### Opinion

Personal preferences, rankings, or subjective judgments.

Examples:

* One Piece has better villains than Naruto.
* Akainu is the best villain in the series.

### Hot_Take
A Hot_Take is a strongly controversial or provocative subjective claim that many One Piece fans would disagree with.

Examples:
- Marineford is overrated.
- Zoro is a badly written character

## 3. Hard Edge Cases

Some posts can fit more than one label. I will label based on the main purpose of the post.

If a post uses a confirmed fact to support a personal judgment, I will label it Opinion.

Example:
“Akainu is the best villain because he killed Ace and went on par with Whitebeard.”
Label: Opinion

If a post makes a controversial subjective claim, I will label it Hot_Take instead of Opinion.

Example:
“Marineford is overrated.”
Label: Hot_Take

If a post predicts something that has not been confirmed, I will label it Theory even if it sounds confident.

Example:
“Akainu’s final opponent will be Koby.”
Label: Theory

If a post contains multiple labels, I will assign the label that best represents the main claim being made.

## 4. Data Collection Plan

I will collect at least 200 comments and discussion posts from the One Piece subreddit and other social networks.  I'll aim for a roughly balanced dataset across Fact, Theory, Opinion, and Hot_Take.
If one label is underrepresented after 200 examples, I will collect more examples specifically for that label instead of leaving the dataset imbalanced.


## 5. Evaluation Metrics

I will evaluate the model using accuracy, precision, recall, F1-score, and a confusion matrix. Accuracy measures overall performance, while precision, recall, and F1-score help evaluate how well the model performs on each label individually. The confusion matrix will help identify which labels are most commonly confused.

## 6. Definition of Success

This classifier will be useful if it can correctly classify most One Piece discussion posts into Fact, Theory, Opinion, or Hot_Take.

My target success threshold is:
- At least 75% overall accuracy
- At least 70% F1-score for each label
- No label should perform extremely worse than the others

If Hot_Take or Theory performs poorly, I will review the mislabeled examples and improve the label definitions or add more training data.

## 7. AI Tool Plan

### Label Stress-Testing
I will use Claude Code to generate 5–10 difficult One Piece posts that sit between two labels, such as Theory vs Opinion or Opinion vs Hot_Take. If I cannot label them clearly, I will tighten my label definitions before training.

### Annotation Assistance
I'll use Groq llama-3.3-70b-versatile as a zero-shot baseline to pre-label examples, but I will manually review every label myself. The final dataset labels will be human-reviewed.

### Failure Analysis
After training, I will give the model’s wrong predictions to Claude Code and ask it to find patterns. I will verify the patterns myself by checking the original examples and confusion matrix.

