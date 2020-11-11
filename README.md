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

1. Setup Entities - Wrap some specific keyword in sq bracket & given them values for

- Syntax -> [entity_name](entity_value)
  for eg [Delhi](location) so whenever delhi come bot will understood it as location

1. Write the Story in the stores.yml file. Stories and rules are both representations of conversations between a user and a conversational assistant.

1. Mention all your intent, entities,slots,forms ,actions & utterance response in domain.yml file

1. Create some custom actions.

## Dialog Policies

- A Poilcies is a component responsible for making a decision of how an assistant should respond next.

### Parameters of Policy

- **max_history** - controls how much dialogue history the model looks at to decide which actions to take next.

Use `max_history` to higher number(recommended is 3) will make chatbot slow so better use slot to store any specific value

- **Data Augmentaion** - an amount of new stories to create from the existing stories.

- **priority** - it defines the priority of the policy.

### Memoization Policy

- It mimics the stories it was trained on. Depending of what max_history parameter was set it tries to match the fragment of the current story with the stories provided in the training data file. If it finds one it predicts the next action from the matched story with a confidence one, otherwise it predicts none with a confidence zero.

### FormPolicy

- It can used to where we want to ask some specilfic details from the user the before taking any specific actions

### Fallback Policy

- When user ask something out of scope then,It allows you to set the threshold for the NLU & dialogue management models & if they are not met then a fallback action is predicted by the fallback policy.

#### Configure the Fallback policy

- name:"FallbackPolciy"
- nlu_threshold: 0.3
- ambiguity_threshold: .3
- core_threshold: 0.3
- fallback_action_name:'action_default_fallback'
