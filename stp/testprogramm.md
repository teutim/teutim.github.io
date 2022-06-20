# Testprogramm

Für STP-1.jar
```java
import de.teutim.stp.*;
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main {

    public static Expression tokenize(String source) {
        return Tokenizer.INSTANCE.tokenizeToExpr(source);
    }

    public static Expression parse(String source) {
        var result = Parser.Companion.getDEFAULT().parse(source);
        return result.hasLeft() ? result.getLeft() : result.getRight();
    }

    public static Expression evaluateHosted(String source) {
        return Expression.Companion.from("{?(parse (io-read-string \"stdlib.stp\")) " + source + "}").evaluate(new Context().setVersionProperty(1));
    }

    public static Expression evaluateFreestanding(String source) {
        return Expression.Companion.from(source).evaluate(new Context().setVersionProperty(1));
    }

    public static void main(String[] args) throws IOException {
        Context.Companion.registerDefaultPrimitives();
        Context.Companion.setScriptsPath("");
        Context.Companion.setOnlinePath("");

        var br = new BufferedReader(new InputStreamReader(System.in));
        String input = "";
        while (!"exit".equals(input)) {
            if (input.startsWith("tokenize ")) {
                System.out.println(tokenize(input.substring("tokenize ".length())));
            } else if (input.startsWith("parse ")) {
                System.out.println(parse(input.substring("parse ".length())));
            } else if (input.startsWith("hosted ")) {
                System.out.println(evaluateHosted(input.substring("hosted ".length())));
            } else if (input.startsWith("free ")) {
                System.out.println(evaluateFreestanding(input.substring("free ".length())));
            } else if (!"".equals(input)) {
                try {
                    System.out.println("= " + evaluateHosted(input));
                } catch (ExitRequest seq) {
                    System.out.println("Script exited");
                }
            }
            System.out.print("> ");
            input = br.readLine();
        }
    }
}

```

Für STP-0.jar
```java
public class Main {
    public static void main(String[] args) {
        Context.registerDefaultPrimitives();
        Context.scriptsPath = "";
        Context.onlinePath = "";

        String code = "";
        boolean multilinemode = false;
        while (true) {
            String input = System.console().readLine("> ");
            if (!multilinemode) code = input;
            if ("exit".equals(input)) {
                break;
            } else if ("{".equals(input)) {
                multilinemode = true;
            } else if ("}".equals(input)) {
                multilinemode = false;
                Context context = new Context().setVersionProperty(1);
                System.out.println("= " + Expression.from("{?(parse (io-read-string \"stdlib.stp\")) " + code + "}}").evaluate(context));
                code = "";
            } else if ("help".equals(input)) {
                System.out.println("= help: Displays a list of commands\n= exit: Exits the program");
            } else {
                if (!multilinemode) {
                    Context context = new Context().setVersionProperty(1);
                    try {
                        System.out.println("= " + Expression.from("{?(parse (io-read-string \"stdlib.stp\")) " + code + "}").evaluate(context));
                    } catch (ExitRequest seq) {
                        System.out.println("Script exited");
                    }
                    code = "";
                } else {
                    code += "\n" + input;
                }
            }
        }
    }
}

```
