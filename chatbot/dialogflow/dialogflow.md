## Google Dialogflow


### Intro

Dialogflow is part of GCP(Google Cloud Platform)'s AI and Machine Learning service. It is made to design and integrate a conversational user interface into your application service. Dialogflow can analyze multiple types of input from your customers, including text or audio inputs (like from a phone or voice recording). It can also respond to your customers in a couple of ways, either through text or with synthetic speech.



### Types

There are two types of services:
- Dialogflow CX: advance agent for large and complex agents 
- Dialogflow ES: standard agent for small and simple agents

Since I am still quite new to dialogflow, I will start with Dialogflow ES and discover step by step.


### Basic flow and components

* `Agent`: handle client side conversations with your end-users. NLP module that transfer text or audio to structure data for later process.
* `Intent`: An intent categorizes an end-user's intention for one conversation turn. Each intent is one scenario. Dialogflow will match each conversation, based on keywords or other information, to match the best intent (intent classification). Intent contains a few key components:
  * Training phrases: phrases that user might said
  * Action: when there is a match, these actions will be trigger
  * Parameters: when an intent is match at runtime, these parameters will be automatically extract out of user conversation
  * Responses: Text, speech, or any visualization that will return to end users.
* `Context`: Like a human conversation, context helps our bot to understand what's this converation is about. 

            -----           -----
 TEXT       | A |           | I |
 ----->     | G |   ----->  | N |   ---> 
 AUDIO      | E |           | T |
            | N |           | E |
            | T |           | N |
            -----           | T |
                            -----

### Reference
- [Basic flow](https://cloud.google.com/dialogflow/es/docs/basics)


