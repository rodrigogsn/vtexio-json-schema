# Frequent used assets and objects for fast copying

## Images

![Mandatory](https://img.shields.io/badge/-Mandatory-red.png)
![Warning](https://developers.vtex.com/img/emojis/warning.png|width=12px)

## Code

#### Block (Example A)

```json
"component": {
  "type": "object",
  "additionalProperties": false,
  "title": "Title",
  "markdownDescription": "https://developers.vtex.com/vtex-developer-docs/docs/\n\nDescription here\n\n`\"vtex.app@0.x\"`",
  "properties": {
    "props": {
      "type": "object",
      "additionalProperties": false,
      "markdownDescription": "All available properties for this block.",
      "properties": {
        "blockClass": {
          "type": ["array", "string"],
          "markdownDescription": "`string | string[]`\n\nBlock container class. This prop’s set value functions as a block identifier for CSS customizations.",
          "items": {
            "type": "string"
          },
          "default": []
        },
        "foobar": {
          "type": "string",
          "markdownDescription": "`enum`\n\n",
          "default": "",
          "oneOf": [
            {
              "const": "",
              "markdownDescription": ""
            },
            {
              "const": "",
              "markdownDescription": ""
            }
          ]
        }
      }
    }
  }
}
```

#### Block (Example B)

```json
"component": {
  "type": "object",
  "additionalProperties": false,
  "title": "Title",
  "markdownDescription": "https://developers.vtex.com/vtex-developer-docs/docs/\n\nDescription here\n\n`\"vtex.app@0.x\"`",
  "properties": {
    "title": {
      "type": "string",
      "markdownDescription": "Define a custom name. It will be shown in site editor for faster identification."
    },
    "children": {
      "type": "array",
      "markdownDescription": "Declare the blocks that must be wrapped inside the current one.",
      "items": {
        "type": "string"
      }
    },
    "props": {
      "type": "object",
      "additionalProperties": false,
      "markdownDescription": "All available properties for this block.",
      "properties": {
        "blockClass": {
          "type": ["array", "string"],
          "markdownDescription": "`string | string[]`\n\nBlock container class. This prop’s set value functions as a block identifier for CSS customizations.",
          "items": {
            "type": "string"
          },
          "default": []
        },
        "foobar": {
          "type": "string",
          "markdownDescription": "`enum`\n\n",
          "default": "",
          "oneOf": [
            {
              "const": "",
              "markdownDescription": ""
            },
            {
              "const": "",
              "markdownDescription": ""
            }
          ]
        }
      }
    }
  }
}
```

#### properties

```json
"foo": {
  "$ref": "#/definitions/foo"
}
```

#### patternProperties

```json
"^foo#": {
  "$ref": "#/definitions/foo"
}
```
