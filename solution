import java.io.*; 
import java.util.*; 
import java.util.stream.*;

public class CandidateCode { 
    public static void main(String[] args) throws IOException { 
        BufferedReader inputBuffer = new BufferedReader(new InputStreamReader(System.in)); 
        List<String> inputLines = inputBuffer.lines().collect(Collectors.toList());

        Map<String, String> equations = new HashMap<>();
        for (String line : inputLines) {
            String[] parts = line.split(" = ");
            equations.put(parts[0], parts[1]);
        }

        String result = processEquations(equations);

        System.out.println(result);
    }

    private static String processEquations(Map<String, String> equations) {
        Set<String> visited = new HashSet<>();
        Set<String> resolved = new HashSet<>();
        Map<String, String> terminals = new HashMap<>();

        for (String lhs : equations.keySet()) {
            if (Character.isDigit(lhs.charAt(0))) {
                return "Error: " + lhs + " - terminals can't be left-hand operand!";
            }
            if (lhs.length() > 3) {
                return "Error: " + lhs + " - nonterminal max length exceeded! (" + lhs + " exceeds 3 letters)";
            }
        }

        for (String lhs : equations.keySet()) {
            if (!resolve(lhs, equations, terminals, visited, resolved)) {
                return "Error: " + lhs + " - self implication not allowed!";
            }
        }

        StringBuilder result = new StringBuilder();
        for (String lhs : equations.keySet()) {
            result.append(lhs).append(" = ").append(terminals.get(lhs)).append("\n");
        }

        return result.toString().trim();
    }

    private static boolean resolve(String lhs, Map<String, String> equations, Map<String, String> terminals, Set<String> visited, Set<String> resolved) {
        if (resolved.contains(lhs)) {
            return true;
        }
        if (visited.contains(lhs)) {
            return false;
        }
        visited.add(lhs);

        String rhs = equations.get(lhs);
        String[] parts = rhs.split(" ");
        StringBuilder terminalForm = new StringBuilder();

        for (String part : parts) {
            if (Character.isDigit(part.charAt(0)) || "+-*/".contains(part)) {
                terminalForm.append(part);
            } else if (equations.containsKey(part)) {
                if (!resolve(part, equations, terminals, visited, resolved)) {
                    return false;
                }
                terminalForm.append(terminals.get(part));
            }
            terminalForm.append(" ");
        }

        terminals.put(lhs, terminalForm.toString().trim());
        resolved.add(lhs);
        visited.remove(lhs);

        return true;
    }
}
