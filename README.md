# USFM Grammar

An elegant [USFM](https://github.com/ubsicap/usfm) parser (or validator) that uses a [parsing expression grammar](https://en.wikipedia.org/wiki/Parsing_expression_grammar) to model USFM. The grammar is written using [ohm](https://ohmlang.github.io/). **Only USFM 3.0 is supported**. 

The parsed USFM is an intuitive and easy to manipulate JSON structure that allows for painless extraction of scripture and other content from the markup. USFM Grammar is also capable of reconverting the generated JSON back to USFM.

## Online Demo!

Try out the usfm-grammar based convertor online: http://usfm.bridgeconn.com/

## Example

### Input USFM String

```
\id hab 45HABGNT92.usfm, Good News Translation, June 2003
\c 3
\s1 A Prayer of Habakkuk
\p
\v 1 This is a prayer of the prophet Habakkuk:
\b
\q1
\v 2 O \nd Lord\nd*, I have heard of what you have done,
\q2 and I am filled with awe.
\q1 Now do again in our times
\q2 the great deeds you used to do.
\q1 Be merciful, even when you are angry.
```

### JSON Output

```
{"metadata":
  {"id":
    {"book":"HAB",
      "details":" 45HABGNT92.usfm, Good News Translation, June 2003"
      }
    },
  "chapters":[
 
    { "header":{"title":"3"},
      "metadata":[
        {"section":{"text":"A Prayer of Habakkuk",
                      "marker":"s1"}},
        {"styling":[{"marker":"p"}]}
        ],
      "verses":[
        { "number":"1 ",
          "metadata":[{"styling":[
                {"marker":"b","index":1},
                {"marker":"q1","index":2}]}],
          "text objects":[
                {"text":"This is a prayer of the prophet Habakkuk:","index":0}],
          "text":"This is a prayer of the prophet Habakkuk: "},
        { "number":"2 ",
          "metadata":[{"styling":[
                {"marker":"q2","index":3},
                {"marker":"q1","index":5},
                {"marker":"q2","index":7},
                {"marker":"q1","index":9}]}],
          "text objects":[
                {"text":"O ","index":0},
                {"nd":[{"text":"Lord"}],
                  "text":"Lord",
                  "closed":true,
                  "inline":true,
                  "index":1},
                {"text":", I have heard of what you have done,","index":2},
                {"text":"and I am filled with awe.","index":4},
                {"text":"Now do again in our times","index":6},
                {"text":"the great deeds you used to do.","index":8},
                {"text":"Be merciful, even when you are angry.","index":10}],
          "text":"O Lord , I have heard of what you have done, and I am filled with awe. Now do again in our times the great deeds you used to do. Be merciful, even when you are angry. "
          }
        ]
      }
    ],
  "messages":{"warnings":["Book code is in lowercase. "]}
}
```

### JSON Output, With Only Scripture Content

```
{"book":"HAB",
 "chapters":[
     {"chapterTitle":"3",
      "verses":[
        {"verseNumber":"1",
          "verseText":"This is a prayer of the prophet Habakkuk: "
          },
        {"verseNumber":"2",
          "verseText":"O Lord , I have heard of what you have done, and I am filled with awe. Now do again in our times the great deeds you used to do. Be merciful, even when you are angry. "
          }
        ]
      }
    ],
 "messages": {"warnings":["Book code is in lowercase. "]}
}
```

## Installation

The parser is [available on NPM](https://www.npmjs.com/package/usfm-grammar) and can be installed by:

`npm install usfm-grammar`

## Usage

#### USFM to JSON
```
var grammar = require('usfm-grammar')
var jsonOutput = grammar.parseUSFM(/**The USFM Text to be converted to JSON**/)
var jsonCleanOutput = grammar.parseUSFM(/**The USFM Text to be converted to JSON**/, grammar.SCRIPTURE)
```
The `grammar.parseUSFM()` method returns a JSON structure for the passed-in USFM string, if it is a valid usfm file.
The `grammar.parseUSFM()` method can take an optional second argument, `grammar.SCRIPTURE`. In which case, the output JSON will contain only the most relevant scripture content, excluding all other USFM content.
If you intent to create a usfm from the data after processing it, we recommend using this method without the `SCRIPTURE` flag as this would loose information of other markers. 


```
var usfmValidity = grammar.validate(/**USFM Text to be checked**/)
```
The `grammar.validate()` method returns a Boolean depending on whether the input USFM text syntax satisfies the grammar or not.

#### JSON to USFM
```
var usfmString = grammar.parseJSON(jsonOutput)
```
The `grammar.parseJSON()` takes a the JSON object generated using USFM Grammar and converts it to it's corresponding USFM text. The only side effect being that multiple spaces in the file are normalized to single-space.

