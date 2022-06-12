# Testprogramm

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
