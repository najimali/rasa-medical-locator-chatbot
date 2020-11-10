# Pipeline

- Processing Pipeline define the NLU model & in what sequence these models are trained & executed
- How the user input is processed
- Which intent specification model & entities extraction models are used to make predictions

Note - Order of pipeline matters most.

## Entities classification Pipeline

- SpacyNLP ,WhitespaceTokenizer,RegexFeaturizer, CRFEntityExtraction, DuckingHttpExtractor

### SpacyNLP

- It is responsible for spatial langauge model & having it ready for the next

### WhitespaceTokenizer

- It splits the sentence into single words by looking for white space in a sentence

### RegexFeaturizer

- To find regular pattern or expression & lookup tables & It is adding additional features to CRFEntityExtractor

### CRFEntityExtraction - Conditional Random Field

- To extract the entities ,like which words from the sentence is entities
- text features - low,title,upper,baised,prefixes,suffixes ,digit etc,

### DucklingHttpExtractor

- To extract the date,zipcode ,phone number,email,time from the sentences.

## Intent classification Pipeline

- CountVectorsFeaturizer(Featurizer), EmbeddingIntentClassifer(Mode)
- SpacyFeaturizer , SklearnIntentClassifier

Featurizer - create new features from token & used in turn in classification model to learn underlying pattern & make the predictions

CountVectorsFeaturizer - It counts the number of words appear in a sentence & we can customize it using **analyzer (char_wb)** & giving min & max anagram(**min_ngram &&max_ngram**)

Flow Diagram -

Input -> WhitespaceFeaturizer -> RegexFeaturizer -> CRFEntityExtraction-> CountVectorsFeaturizer ->EmbeddingIntentClassifier ->Output

### Advantage of using EmbeddingIntentClasssifier:

- It learn from scratch so it is domain specified & handle the domain specific vocabulary

- Build the AI system in any language that can be tokenized

- Can handle the messages with multple intents

## Steps to create bot [Docs for training data](https://rasa.com/docs/rasa/training-data-format#conversation-training-data)

1. Define the pipeline you want to use in your project

1. Define the intent in the nlu.yml file for eg i want to set intent for inform

- intent: inform
  examples: | - [Delhi](location) - [Mumbai](location) - [Jaipur](location) - [Hamipur](location) - [Banglore](location) - [Noida](location) - [Chennai](location) - [Mainpuri](location) - [Makrana](location)

intent :intent_name_1
examples: | - eg 1 for intent_name_1 - eg 2 - eg 3

1. Wrap some specific keyword in sq bracket & given them values for

- Syntax -> [entity_name](entity_value)
  for eg [Delhi](location) so whenever delhi come bot will understood as location

1. Write the Story in the stores.yml file. Stories and rules are both representations of conversations between a user and a conversational assistant.
