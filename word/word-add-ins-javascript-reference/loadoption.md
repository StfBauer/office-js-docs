# LoadOption object (Javascript API for Word)

An object that specifies paging information and the properties to load when context.sync() is called. 

## Properties
| Property	   | Type	|Description|
|:---------------|:--------|:----------|
|select|object|Contains a comma delimited list or an array of parameter/relationship names. Optional.|
|expand|object|Contains a comma delimited list or an array of relationship names. Optional.|
|top|int| Specifies the maximum number of collection items that can be included in the result. Optional.|
|skip|int|Specify the number of items in the collection that are to be skipped and not included in the result. If `top` is specified, the result set will start after skipping the specified number of items. Optional.|

## More information

The preferred method for specifying the properties and paging information is by using a string literal. The first two examples show the preferred way to request the text and font size properties for paragraphs in a paragraph collection:

<code>context.load(paragraphs, 'text, font/size, top: 50, skip: 0');</code>

<code>paragraphs.load('text, font/size, top: 50, skip: 0');</code>

Here is the equivalent using object notation:

<code>context.load(paragraphs, {select: 'text, font/size',
                                expand: 'font',
                                top: 50,
                                skip: 0});</code>
                                
<code>paragraphs.load({select: 'text, font/size',
                       expand: 'font',
                       top: 50,
                       skip: 0});</code>

Note that if we don't specify the specific properties on the font object in the select statement, the expand statement by itself would indicate that all of the font properties are loaded. 

## Examples

This example shows how to get the top 50 paragraphs in the Word document along with their text and font size properties.

```js
        // Run a batch operation against the Word object model.
        Word.run(function (context) {

            // Create a proxy object for the paragraphs collection.
            var paragraphs = context.document.body.paragraphs;

            // Queue a commmand to load the text and font properties for the top 50 paragraphs.
            // It is best practice to always specify the property set. Otherwise, all properties are
            // returned in on the object. 
            context.load(paragraphs, 'text, font/size, top: 50, skip: 0');

            // Synchronize the document state by executing the queued commands, 
            // and return a promise to indicate task completion.
            return context.sync().then(function () {
            
            // Insert code that works with the paragraphs loaded by context.load().

        })
        .catch(function (error) {
            console.log('Error: ' + JSON.stringify(error));
            if (error instanceof OfficeExtension.Error) {
                console.log('Debug info: ' + JSON.stringify(error.debugInfo));
            }
        });

```