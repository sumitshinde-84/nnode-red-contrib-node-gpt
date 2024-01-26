## overview
Inquires ChatGPT from the payload string.

## install

````
npm i node-red-contrib-simple-chatgpt
````

or

Install from the Admin tab

## How to use
### Input items

|Item|Description|
|--|--|
|Token|Set the API key for OpenAPI, also you can set token dynamically by msg.token |
|Model|Specifies the model name to use. The default is `gpt-3.5-turbo`. |
|SystemSetting|Describe the AI assistant settings.|
|pastMessages|Passes conversation history. Required to continue the conversation. |
|functions|Can be used from `gpt-3.5-turbo-0613` or later. Specified samples are sold separately. |
You can forcibly execute the function name specified by |function_call|functions. If you set it to `auto`, the function will be automatically determined and called. If it is `none`, it will not be called. Specifying `{name: function name}` will force execution of the target function. |

### Output items

|Item|Description|
|--|--|
|payload|You will receive a ChatGPT response. If the function is executed, `null` is returned. |
|pastMessages|Returns a history array of conversations. |
When executed with |payloadFunction|FunctionCalling, the executed function name and JSON parsed arguments are returned. |

## How to specify Functions
A sample of Functions is shown below. Specify the function name, function details, and parameters in array format.

|Item|Description|
|--|--|
|name|Name of the function. You can choose any name you like. |
|description|Detailed description of the function. It is preferable to write in some detail. |
|parameters.properties|Parameters details. Enter the property name, type, and description you want to set. |
|parameters.required| Specify the required property name that you want returned in the property. |

```json
[
     {
         "name": "get_weather",
         "description": "Get the weather for the specified location and date",
         "parameters": {
             "type": "object",
             "properties": {
                 "location": {
                     "type": "string",
                     "description": "Name of prefecture, city, or town, e.g. London"
                 },
                 "date": {
                     "type": "string",
                     "description": "Date formatted in YYYY/MM/DD, e.g. 2023/06/13"
                 }
             },
             "required": [
                 "location",
                 "date"
             ]
         }
     },
     {
         "name": "recommend_book",
         "description": "Introduce one recommended book",
         "parameters": {
             "type": "object",
             "properties": {
                 "title": {
                     "type": "string",
                     "description": "Book title"
                 },
                 "description": {
                     "type": "string",
                     "description": "Book contents"
                 }
             },
             "required": [
                 "title",
                 "description"
             ]
         }
     },
     {
         "name": "hashtag_text",
         "description": "Output a hashtag from the text provided by the user.",
         "parameters": {
             "type": "object",
             "properties": {
                 "tag": {
                     "type": "string",
                     "description": "Please output at least 3 hashtags."
                 }
             },
             "required": [
                 "tag"
             ]
         }
     }
]
````

For details on how to specify, please see [here](https://openai.com/blog/function-calling-and-other-api-updates).

## How to specify function_call
In addition to `auto` or `none` as a string, specify the name of the function to be forcibly executed on the Json object.
This is an example when the above Functions are given. `hashtag_text` is now forced.

```json
{
     "name": "hashtag_text"
}