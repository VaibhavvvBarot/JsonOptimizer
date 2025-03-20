# Logic JSON Optimizer

A Python utility for optimizing JSON structures for logic control and logic expression objects. This tool removes unnecessary keys from complex nested JSON structures while preserving the essential fields required for different logic action types.

## Overview

This tool is designed to work with JSON objects that follow specific logic action schemas:
- Assign Value actions
- Conditional actions (with nested true/false branches)
- UI control actions (Hide Fields, Show Fields, Enable Fields)

The optimizer automatically:
- Preserves the required LogicControl keys
- Preserves the required LogicExpression keys
- Maintains nested structure relationships
- Removes extraneous fields that bloat the JSON

## Installation

### Requirements
- Python 3.6+
- Google Colab (for the web interface)

### Local Setup
1. Clone this repository:
   ```
   git clone https://github.com/VaibhavvvBarot/JsonOptimizer.git
   cd JsonOptimizer
   ```

2. Install dependencies:
   ```
   pip install -r requirements.txt
   ```

## Usage

### In Google Colab

1. Upload the script to Google Colab
2. Run the cell containing the script
3. When prompted, paste your JSON
4. The optimized JSON will appear in a text area for easy copying

### Local Python Environment

```python
from json_optimizer import optimize_logic_action
import json

# Load your JSON
with open('input.json', 'r') as file:
    input_json = json.load(file)

# Optimize it
optimized_json = optimize_logic_action(input_json)

# Save the result
with open('optimized.json', 'w') as file:
    json.dump(optimized_json, file, indent=2)
```

## Key Structures

The optimizer preserves the following key structures:

### LogicControl Keys
```
ValueType
DocSourceName
Value
DataType
ValueSubType
DataSubType
LookupType
LookupName
```

### LogicExpression Additional Keys
```
ConditionalOperator
ID
ExpressionType
CompareToValue
CompareToValueType
CompareToDocSourceName
CompareToDataType
CompareToDataSubType
CompareToLookupType
LogicalOperator
```

### Action-Specific Keys
- For "Assign Value" outputs: `AssignAction`
- For UI control actions: `Index`

## Examples

### Example: Optimizing "Assign Value" Action

**Input JSON:**
```json
{
  "logicAction": "Assign Value",
  "logicInstanceInputs": [
    {
      "Value": "BA",
      "DataType": "Text",
      "ValueType": "Constant",
      "LookupName": "NWTUnits of Measure",
      "LookupType": "Single",
      "DocSourceName": "",
      "ExtraKey1": "Will be removed",
      "ExtraKey2": "Will be removed"
    }
  ],
  "logicInstanceOutputs": [
    {
      "Value": "newLL",
      "DataType": "Text",
      "ValueType": "Variable",
      "LookupName": "NWTUnits of Measure",
      "LookupType": "Single",
      "AssignAction": "set",
      "ExtraKey3": "Will be removed"
    }
  ],
  "ExtraTopLevelKey": "Will be removed"
}
```

**Output JSON:**
```json
{
  "logicAction": "Assign Value",
  "logicInstanceInputs": [
    {
      "Value": "BA",
      "DataType": "Text",
      "ValueType": "Constant",
      "LookupName": "NWTUnits of Measure",
      "LookupType": "Single",
      "DocSourceName": ""
    }
  ],
  "logicInstanceOutputs": [
    {
      "Value": "newLL",
      "DataType": "Text",
      "ValueType": "Variable",
      "LookupName": "NWTUnits of Measure",
      "LookupType": "Single",
      "AssignAction": "set"
    }
  ]
}
```

### Example: Optimizing "Conditional" Action

The optimizer also handles deeply nested conditional structures with true/false branches containing additional logic actions.

## How It Works

The tool uses a recursive approach to process nested JSON structures:

1. It identifies the logicAction type of each object
2. Applies appropriate filtering rules based on the action type
3. Recursively processes any nested logic actions
4. Preserves the structure of arrays and object hierarchies
5. Returns the optimized JSON with only the necessary keys

## License

MIT License

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.
