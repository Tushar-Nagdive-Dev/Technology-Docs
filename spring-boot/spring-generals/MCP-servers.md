**The Restaurant Analogy (Simple Terms)**
Imagine an AI (like Claude or ChatGPT) is a brilliant Master Chef who knows every recipe in the world. However, they are locked in a glass office. They know exactly *how* to cook, but they physically cannot open the fridge or turn on the oven.

**MCP (Model Context Protocol)** is the universal "Waiter" and ordering system that lets the Chef safely talk to the Kitchen Staff (your application). 

When you build an **MCP Server**, you are handing the AI a "Menu" of specific "Tools" it is allowed to use. So when a user tells the AI to "Add milk to my cart", the AI uses MCP to say, *"Hey Kitchen, execute the `addToCart` tool with the parameter 'milk'"*. 

Before MCP, you had to write custom, messy API integrations for every different AI model. Now, MCP acts as the universal bridge.

---

### How it works in Spring Boot

Spring AI has fully embraced MCP. You can now take a normal Spring Boot application and turn it into an MCP server by simply adding a dependency and using a few annotations. Spring handles all the complex JSON-RPC communication (over STDIO or HTTP) completely under the hood.

Here is a real, working example of a Shopping Cart MCP Server.

**1. Add Dependencies**
We use the WebMVC Streamable starter to communicate via HTTP.
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-mcp-server-webmvc</artifactId>
</dependency>
```

**2. Configure the Server**
In your `application.properties`, you tell Spring to run the MCP server over a Streamable HTTP protocol.
```properties
spring.application.name=shopping-mcp
spring.ai.mcp.server.name=shopping-cart-server
spring.ai.mcp.server.version=1.0.0
spring.ai.mcp.server.protocol=STREAMABLE
```

**3. The Java Code**
You just write a normal Spring `@Service` and use the `@McpTool` annotation. Spring AI will automatically scan this, figure out what the method does based on your descriptions, and expose it to the AI model.

```java
import org.springframework.ai.mcp.server.annotation.McpTool;
import org.springframework.ai.mcp.server.annotation.McpToolParam;
import org.springframework.stereotype.Service;

import java.util.ArrayList;
import java.util.List;

@Service
public class ShoppingCartService {

    private final List<String> cart = new ArrayList<>();

    // This annotation tells the AI: "Here is a tool you can use!"
    @McpTool(description = "Add an item to the user's shopping cart")
    public String addToCart(
            @McpToolParam(description = "The name of the product to add") String product,
            @McpToolParam(description = "The quantity to add") int quantity) {
        
        for (int i = 0; i < quantity; i++) {
            cart.add(product);
        }
        
        return quantity + "x " + product + " added successfully. Cart total items: " + cart.size();
    }

    @McpTool(description = "Get all current items in the shopping cart")
    public List<String> getCartItems() {
        return cart;
    }
}
```

**What happens next?**
If you connect an MCP Client (like the Claude Desktop app or another Spring AI app) to this server, the user can literally just type: *"I need to buy 3 apples."*
The AI will understand the intent, realize it has the `addToCart` tool on its menu, extract "apples" and "3" from the sentence, and automatically trigger your Java method!

[![Preview](https://img.shields.io/badge/🚀-Preview-blue?style=for-the-badge)](https://htmlpreview.github.io/?https://github.com/Tushar-Nagdive/StackBlueprint/blob/StackTech/spring-boot/spring-generals/visuals/mcp_master_guide.html)