# APEX Office Print (AOP) Python SDK
This project provides a Python interface for [APEX Office Print](https://www.apexofficeprint.com), a template based print and export server.
APEX Office Print server can run in the cloud as a service or on-premises.

Create your template in Word, Excel, Powerpoint, HTML, Text or Markdown in which you use {tags}. AOP will replace those tags with the data you pass to AOP.

## Example
```python
import apexofficeprint as aop

TEMPLATE_PATH = "./template.docx"
LOCAL_SERVER_URL = "https://api.apexofficeprint.com"
API_KEY = "YOUR_API_KEY"

def basic_test():
    server = aop.config.Server(
        LOCAL_SERVER_URL, aop.config.ServerConfig(api_key=API_KEY))
    template = aop.Resource.from_local_file(TEMPLATE_PATH)

    data1 = aop.elements.Object("data1")
    imageElement = aop.elements.Image.from_file("imageTag", "./test.jpg")
    imageElement.max_width = 500
    imageElement.rotation = 75
    data1.add(imageElement)

    data2 = aop.elements.Object("data2")
    data2.add_all(aop.elements.Object.from_mapping({
        "textTag1": "Hello",
        "textTag2": ", ",
        "textTag3": "world",
        "textTag4": "!"
    }))

    # create a print job with default output config
    printjob = aop.PrintJob(template, {
        "output1": data1,
        "output2": data2
    }, server)

    try:
        res = printjob.execute()
        print("Success!")
        res.to_file("./output")
    except aop.exceptions.AOPError as err:
        print("AOP error occurred! Encoded message below:")
        print(err.encoded_message)
```

## Documentation
The generated documentation can be found here: [https://www.apexofficeprint.com/docs/python/](https://www.apexofficeprint.com/docs/python/).

[pdoc](https://pdoc3.github.io/pdoc/) is used for documentation generation.
Things to keep in mind when writing docs (some of these are non-standard):
- Docstrings are inherited from `super()`.
- Instance variables (attributes) can have docstrings, start the docstring on the line *underneath* the attribute
- For `@property` properties, the setter's documentation is ignored. Make sure everything is in the getter.
- You can use markdown in the docstrings, along with the generated google-style docs.
  - Doing something like \``ClassName`\` (with backticks, which are generally for inline code) makes pdoc look for the reference and try to hyperlink it in the generated docs.</br>
  This works with any object, useful methods or instance variables of a class too. Maked pdoc look for that 

### Useful VS Code extensions
- `njpwerner.autodocstring`: Python docstring generator, uses Google-style docs by default.

## Contributing
We encourage everyone to participate in the development of the SDK for Python.

## Support
This project is supported by United Codes. Feel free to contact support at support@apexofficeprint.com