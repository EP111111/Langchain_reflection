# Langchain_reflection

## Introduction

The `langchain` package provides utility functions for interacting with language models like vertex ai Palm for various tasks such as reflection and chat interactions. .

## `reflection` Function

The `reflection` function is designed to determine if a given pattern is present in a given text and provide an explanation for the presence or absence of the pattern. It takes two arguments: `des` (pattern description) and `text` (the text for evaluation).

### Function Signature

```python
def reflection(des, text):
    template = """Description: {des}
    
      Answer: Based on above pattern description and example cases above, determine if the pattern  is presented in the following text: 
      “{text_for_evaluation}”
    
      Provide a response comprising a result and an explanation for why the pattern is or is not present in JSON format. The explanation should be around 30 words.
    
      Example output:
    
      {{  "pattern presented" : “Yes” or “No”,
        “Explanation”: “The explanation goes here”  
      }}"""
    
      prompt = PromptTemplate(template=template,input_variables=["des","text_for_evaluation"])
      feed=prompt.format(des=des,text_for_evaluation=text)
```


## Parameters
des: A string describing all info that needs to be checked in the text.
text: The input text in which the presence of the pattern needs to be checked.
## How It Works
1. The function starts by defining a template string that includes placeholders for the pattern description and the text to be evaluated. This template is formatted in JSON format to match the expected response.
2. A PromptTemplate instance is created using the defined template and input variables (des and text_for_evaluation).
3. The template is filled with the provided des and text using the .format() method to create the feed for the language model.
4. The function sends the feed to the language model using the llm function, which is assumed to be available in the context.

## chat_models Function
The chat_models function facilitates a chat-like interaction with the language model to determine the presence of a pattern in a given text. It uses Palm for this purpose. It takes two arguments: des (pattern description) and text (the text for evaluation).

## Function Signature
```
def chat_models(des,text):
  chat = ChatVertexAI()
  template = ChatPromptTemplate.from_messages([
    ("system", des),
    ("human", """Based on above pattern description and example cases above, determine if the pattern  is presented in the following text: 
  “{text_for_evaluation}”

  Provide a response comprising a result and an explanation for why the pattern is or is not present in JSON format. The explanation should be around 30 words.

  Example output:

  {{  "pattern presented" : “Yes” or “No”,
    “Explanation”: “The explanation goes here”  
  }}"""),
    ])


  messages = template.format_messages(
    text_for_evaluation=text
  )
  return chat(messages)
```

## Parameters
des: A string describing the pattern for context in the conversation.
text: The text in which the presence of the pattern needs to be checked.

## How It Works
1. The function starts by creating a chat template using the ChatPromptTemplate class. This template involves a conversation between the system and a human, where the system provides the pattern description, and the human responds with the evaluation text.
2. The template is formatted using the provided des and a placeholder for the evaluation text.
3. The chat interaction is defined by a system message followed by a human message in the template.
