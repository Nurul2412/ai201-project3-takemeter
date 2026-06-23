# ai201-project3-takemeter

## 1. Introduction
TakeMeter: One Piece Community Post Classification
Project Overview

TakeMeter is a text classification system that categorizes One Piece community posts into four categories:

Opinion
Fact
Theory
Hot_Take

The goal is to distinguish between factual statements, personal opinions, speculative theories, and controversial takes commonly found in online One Piece discussions.

## 2. Why this community

I chose the ongoing One Piece manga/anime community because One Piece is my favorite anime. After starting watching One Piece I frequently started seeing One Piece posts across all of my social networks. On those posts I've seen several discussions about One Piece such as fans debating among themselves about a hot take, fans making theories what might happen later on in the series, talks about story events, fans reactions and opinion about the manga/anime and many more. This frequent discussions of the series makes it a good source for diverse discourse.

## 3. Label Taxonomy

Opinion: A personal preference, feeling, or subjective judgment.

Examples:

"Zoro is the best Straw Hat."
"Dressrosa is underrated."

Fact: A statement that can be verified using information from the manga, anime, SBS, or official sources.

Examples:

"Luffy ate the Hito Hito no Mi, Model: Nika."
"Kaido was introduced in Wano."

Theory: A prediction or speculation about future events that are not confirmed.

Examples:

"Blackbeard will obtain a third Devil Fruit."
"Imu is connected to Joyboy."

Hot_Take: A controversial or intentionally provocative opinion that many fans would strongly disagree with.

Examples:

"Marineford is overrated."
"Gear5 is one of worst transformation ever."

## 4. Data Set

Data Source

Posts were manually collected from One Piece reddit discussion communities and fan conversations.

Dataset Size
Total examples: 200+

Labels:
Opinion
Fact
Theory
Hot_Take

Label Distribution

| Label    | Count |
| -------- | ----- |
| Opinion  | 51    |
| Fact     | 60    |
| Theory   | 50    |
| Hot_Take | 50    |

### Difficult Labeling Decisions

Example 1

Post: "Akainu's final opponent will be Koby."

Label: Theory

Reason:

The statement predicts a future event that has not been confirmed.

Example 2

Post: "Dressrosa is basically a mirrored version of Alabasta."

Label: Opinion

Reason:

Although it references story structure, it reflects an interpretation rather than a verifiable fact.

Example 3

Post: "Kaido doesn't get enough hate."

Label: Hot_Take

Reason:

The statement is intentionally controversial and designed to challenge common community sentiment.

## 5. Fine-Tuning Pipeline

Base Model

DistilBERT

Training Platform

Google Colab

Training Decision

Training Decision

I fine-tuned DistilBERT for 6 epochs on the labeled dataset using Google Colab.

During training, validation accuracy improved from approximately 25% in the early epochs to about 72% by the final epoch, showing that the model was learning the label patterns in the dataset.

The goal was to learn distinctions between highly similar categories such as Opinion and Hot_Take, and Fact and Theory.

## 6. Baseline Model

Model

Groq LLM Zero-Shot Classifier

Prompt

The baseline model was given a post and asked to classify it into one of the four labels.

Results Collection

Predictions were collected on the same test set used for the fine-tuned model.

## 7. Evaluation Results

### Overall Accuracy
Model	Accuracy
Groq Zero-Shot Baseline	87.5%
Fine-Tuned DistilBERT	62.5%


### per-class metrics (fine-tuned model):
              precision    recall  f1-score   support

     Opinion       0.60      0.75      0.67         8
        Fact       0.78      0.78      0.78         9
      Theory       0.80      0.50      0.62         8
    Hot_Take       0.38      0.43      0.40         7

    accuracy                           0.62        32
   macro avg       0.64      0.61      0.61        32
weighted avg       0.65      0.62      0.63        32

### Confusion Matrix


| True \ Predicted | Opinion | Fact | Theory | Hot_Take |
| ---------------- | ------- | ---- | ------ | -------- |
| Opinion          | 6       | 0    | 0      | 2        |
| Fact             | 1       | 7    | 1      | 0        |
| Theory           | 0       | 1    | 4      | 3        |
| Hot_Take         | 3       | 1    | 0      | 3        |


## 8. Error Analysis

### Error 1

Post:
The actual One piece" is the tree or source of all devil fruits"

True Label: Theory
Predicted Label: Fact

Analysis:
This statement presents an unconfirmed idea as if it were already true. The post does not contain obvious theory keywords such as "I think", "maybe", or "could be", which likely caused the model to interpret it as a factual claim. The model relied on the confident wording of the sentence rather than recognizing that the claim is speculative and has never been confirmed in the story. This suggests the model struggles when theories are written as definitive statements.

### Error 2

Post:
"Joyboy never lost. He became Imu."

True Label: Theory
Predicted Label: Hot_Take

Analysis:
The statement is speculative but written with strong confidence, causing confusion between Theory and Hot_Take.

### Error 3

Post:
"Kaido doesn't get enough hate."

True Label: Hot_Take
Predicted Label: Opinion

Analysis:
The model identified the statement as subjective but failed to recognize its intentionally controversial nature.

## 9. Reflection

The model performed reasonably on Fact and Opinion posts but struggled with Theory and Hot_Take.

Many theories are written confidently, making them appear factual. Similarly, some hot takes resemble ordinary opinions, making the boundary difficult to learn.

The model appears to rely heavily on wording and confidence rather than deeper context.

To improve performance, I would collect more examples specifically targeting Theory vs Hot_Take edge cases.

## 10. Simple Classification


| Post | Prediction |
|--------|--------|
| Luffy defeated Kaido. | Fact |
| "Luffy will have a disease similar to Roger in the future, but Chopper will make a medicine that'll cure Luffy",Theory | Theory |
| Marineford is overrated. | Hot_Take |

## 11. Spec Reflection

The original project specification helped me define the taxonomy before collecting data. While labeling posts, I discovered that the distinction between Theory and Hot_Take was more difficult than expected because both categories often contain unsupported claims and subjective reasoning.

The biggest challenge was maintaining consistent labels across similar posts. Through error analysis, I found that many model mistakes occurred because theories were written as confident statements and hot takes often resembled normal opinions.

If I continued this project, I would collect additional examples specifically focused on Theory vs Hot_Take boundary cases and provide clearer annotation guidelines for controversial opinions.

## 11. AI Usage

### 1

I used ChatGPT to analyze model errors and identify recurring confusion patterns between Theory and Hot_Take. This helped me understand why certain posts were misclassified.

### 2
 I used Gemini inside Google Colab to debug a Groq API connection issue. Gemini helped identify that the API key contained hidden newline characters, which prevented successful requests to the Groq API. After removing the extra whitespace and reinitializing the client, the baseline evaluation worked correctly.

### 3 

I used claude code to fix any typos in my labeled_dataset_clean.csv dataset

## Additional Edge Case

During baseline evaluation, Groq occasionally returned the misspelled label "opinon" instead of "opinion". A normalization step was added before evaluation.