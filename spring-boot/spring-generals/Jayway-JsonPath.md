Think of **Jayway JsonPath** as the **"SQL" or "XPath" for JSON**. 

When you have a massive, complex JSON response from an API, parsing it using standard Java objects (like creating dozens of POJOs or using heavy `ObjectMapper` trees) can be exhausting. JsonPath allows you to write a simple string query to extract exactly what you need, instantly.

Here is a breakdown of how it works and the most useful methods you will need.

---

### 1. The Setup (Maven Dependency)
First, you need to add the library to your `pom.xml`:
```xml
<dependency>
    <groupId>com.jayway.jsonpath</groupId>
    <artifactId>jsonpath</artifactId>
    <version>2.9.0</version> <!-- Use the latest version -->
</dependency>
```

### 2. The Two Ways to Read JSON
There are two main ways to use the library, depending on performance needs:

*   **One-Off Reading (`JsonPath.read`):** Good if you only need to extract one single value. It parses the JSON, finds the value, and throws the parsed tree away.
*   **Parse Once, Read Many (`JsonPath.parse`):** If you need to extract 5 different things from the same JSON, you should parse it into a `DocumentContext` first. This is much faster.

### 3. Java Code Example: All Useful Methods
Here is a complete, runnable example showing how to extract data and even **modify** the JSON structure directly.

```java
import com.jayway.jsonpath.DocumentContext;
import com.jayway.jsonpath.JsonPath;
import java.util.List;
import java.util.Map;

public class JsonPathMasterClass {

    public static void main(String[] args) {
        // Sample JSON string
        String json = """
        {
          "store": {
            "book": [
              { "category": "reference", "author": "Nigel", "title": "Sayings", "price": 8.95 },
              { "category": "fiction", "author": "Evelyn", "title": "Sword", "price": 12.99 }
            ],
            "bicycle": { "color": "red", "price": 19.95 }
          }
        }
        """;

        // ==========================================
        // 1. EXTRACTING DATA (READ)
        // ==========================================
        
        // Parse the JSON once for better performance
        DocumentContext context = JsonPath.parse(json);

        // A. Simple Path: Get the first book's author
        String author = context.read("$.store.book[0].author");
        System.out.println("First Author: " + author);

        // B. Wildcard [*]: Get ALL book titles
        List<String> titles = context.read("$.store.book[*].title");
        System.out.println("All Titles: " + titles);

        // C. Deep Scan [..]: Find ALL 'price' keys anywhere in the document
        List<Double> allPrices = context.read("$..price");
        System.out.println("All Prices: " + allPrices);

        // D. Filters [?(...)]: Find books cheaper than $10
        List<Map<String, Object>> cheapBooks = context.read("$.store.book[?(@.price < 10)]");
        System.out.println("Cheap Books: " + cheapBooks);


        // ==========================================
        // 2. MODIFYING DATA (WRITE)
        // ==========================================

        // E. set(): Update an existing value
        context.set("$.store.bicycle.color", "blue");

        // F. put(): Add a new Key-Value pair to an Object
        // (Path to the object, New Key, New Value)
        context.put("$.store.bicycle", "brand", "Trek");

        // G. add(): Add a new element to an Array
        context.add("$.store.book", Map.of("title", "New AI Book", "price", 25.00));

        // H. delete(): Remove a node entirely
        context.delete("$.store.book[0]");

        // Print the newly modified JSON
        System.out.println("\nModified JSON: " + context.jsonString());
    }
}
```


[![Preview](https://img.shields.io/badge/🚀-Preview-blue?style=for-the-badge)](https://htmlpreview.github.io/?https://github.com/Tushar-Nagdive/StackBlueprint/blob/StackTech/spring-boot/spring-generals/visuals/jayway_jsonpath.html)