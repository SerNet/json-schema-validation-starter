# Json-Schema-Validation-Starter

This provides a Spring-Boot-Starter to include JsonSchemaValidation with the help of the [https://github.com/networknt/json-schema-validator](https://github.com/networknt/json-schema-validator) -library.

## Usage

Include the starter into you're project.

You need to add jitpack to your `pom.xml` because this project is not available in the official maven repository.
```
<repositories>
    <repository>
        <id>jitpack.io</id>
        <url>https://jitpack.io</url>
    </repository>
</repositories>
```

Add the `json-schema-validation-starter`-dependency to your `pom.xml`
```
<dependency>
    <groupId>com.github.JanLoebel</groupId>
    <artifactId>json-schema-validation-starter</artifactId>
    <version>2.2.1</version>
</dependency>
```

After that simply create a json-schema and put it into e.g.: `resources/jsonschema/book.json`.
The last step is now to let your entity know that it should be validated and which schema it should use.

```
@JsonSchemaValidation("classpath:jsonschema/book.json")
public class Book {
    private String title;
    private String author;
}
```

Alternatively, you need to add this annotation in your controller like below. This is helpful when you generate your classes from a schema or can't edit them.

```
@RestController
@RequestMapping("/books")
public BooksConroller {
     @PostMapping
     public ResponseEntity<Book> createBook(@RequestBody @JsonSchemaValidation("classpath:jsonschema/book.json") Book bookDto) {
        //...
        return bookDto;
    }
}
```

The schemas are read by a bean that implements the `JsonSchemaProvider` interface. By default, the `DefaultJsonSchemaProvider` is used, which can load schemas by a URL or from the classpath. If necessary, a custom schema provider can be implemented and configured:

```
@ConditionalOnProperty(prefix = "json.schema.validation", name = "schemaProvider", havingValue = "custom")
@Component
public class CustomJsonSchemaProvider implements JsonSchemaProvider {
     JsonSchema loadSchema(String url) {
         // Create and return a JSON schema...
     }

    void handleValidationMessages(Collection<ValidationMessage> validationMessages) {
         // Handle validation messages...
    }
}
```

Configure the schema provider in `application.properties`:

```
json.schema.validation.schemaProvider=custom
```

## Example project
Head over to [http://github.com/JanLoebel/json-schema-validation-starter-example](http://github.com/JanLoebel/json-schema-validation-starter-example) to checkout the sample project.

## Contribution
Please feel free to improve or modify the code and open a Pull-Request! Any contribution is welcome :)

## License
MIT License

Copyright (c) 2019 Jan Löbel

See LICENSE file for details.
