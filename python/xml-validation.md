# Validation of XML documents

You can validate XML files can be against their schema `xsd` files with the following code:
```python
import xmlschema
# Create schema object
try:
    schema = xmlschema.XMLSchema(schema_path)
    schema.validate(xml_path)
    return True, "XML is valid according to the schema."

except xmlschema.XMLSchemaValidationError as e:
    return False, f"Validation error: {str(e)}"
except xmlschema.XMLSchemaParseError as e:
    return False, f"Schema parsing error: {str(e)}"
except Exception as e:
    return False, f"Unexpected error: {str(e)}"
```

This will give you a very good indication whether the XML file is valid or not. If the XML file is not valid, the error message will give you a good indication of what is wrong.
